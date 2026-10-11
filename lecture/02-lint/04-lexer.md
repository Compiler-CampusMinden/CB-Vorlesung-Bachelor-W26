# L-Int: Lexer - Handcodierte Implementierung

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> Der Lexer (auch "Scanner") soll den Zeichenstrom in eine Folge von Token zerlegen. Zur Spezifikation der Token werden reguläre Ausdrücke verwendet.
>
> Von Hand implementierte Lexer arbeiten üblicherweise iterativ und durchlaufen die Eingabe schrittweise von links nach rechts. Dabei verarbeiten immer das aktuelle Zeichen im Eingabestrom. Die Arbeitsweise erinnert an LL-Parser.
>
> Lexer sollten sehr effizient sein, da sie noch direkt auf der niedrigsten Abstraktionsstufe arbeiten und u.U. oft durchlaufen werden.
>
> Die Palette an Fehlerbehandlungsstrategien im Lexer reicht von "aufgeben" über den "Panic Mode" ("gobbeln" von Zeichen, bis wieder eines passt) und Ein-Schritt-Transformationen bis hin zu speziellen Lexer-Regeln, die beispielsweise besonders häufige Typos abfangen.
>
> </details>

## Lexer: Erzeugen eines Token-Stroms aus einem Zeichenstrom

Aus dem Eingabe(-quell-)text

``` clojure
;; demo
(+ 1 222 )
```

erstellt der Lexer (oder auch Scanner genannt) eine Sequenz von Token:

    <LPAREN, "("> <PLUS, "+"> <INT, "1"> <INT, "222"> <RPAREN, ")">

Wir wollen den Eingabetext in abstraktere Einheiten aufsplitten: die Token.

Jedes Token hat einen Typ und speichert ein Lexem - das ist der Teil der Eingabe, der zu dem Token gematcht wurde.

Der Lexer versucht anhand von Regeln (reguläre Ausdrücke) zu erkennen, welche Token in der Eingabe vorkommen. In der Regel geht man zeichenweise von links nach rechts durch die Eingabe und versucht anhand des aktuellen Zeichens zu erkennen, welches Token hier beginnt. Bei Mehrdeutigkeiten muss man entsprechend noch weitere folgende Zeichen mit einbeziehen.

## Modellierung der Token

``` ebnf
eform ::= '(' '+' eform eform+ ')' | '(' '-' eform eform+ ')' | INT
INT   ::= [0-9]+
```

``` java
public enum TokenKind {
    PLUS, MINUS, LPAREN, RPAREN, SEMI,
    INT, // [0-9]+
    EOF
}

public record Token(TokenKind kind, String lexeme) {}
```

> [!TIP]
>
> ### Design-Diskussion: Modellierung Token
>
> Es gibt verschiedene Möglichkeiten, Token zu modellieren.
>
> Aus der C-Welt stammt die gezeigte Modellierung: Man nutzt ein Enum, um die Tokenarten zu modellieren. (Wir werden dieses Enum im Laufe des Semesters noch deutlich ergänzen.) Für die Repräsentation der Token nutzt man dann eine gemeinsame Daten-Klasse, die einfach die Tokenart und das Lexem kombiniert. Das ist übersichtlich, allerdings muss man für Token wie `PLUS` das Lexem "+" dann nochmal explizit mit speichern.

> [!IMPORTANT]
>
> ### Weitere Token und Eigenschaften
>
> Token enthalten oft auch Positionen oder Quelltextbereiche (Zeile, Spalte, Start- und Endindex). Das erleichtert Fehlermeldungen erheblich.
>
> `EOF` ist ein synthetisches Token und hat kein Lexem aus dem Quelltext. "<EOF>" ist eine mögliche Debug-Darstellung, aber kein tatsächliches Lexem.

## Grundstruktur eines Lexers

``` java
public abstract class BaseLexer {
    protected String source;
    protected int current;

    public List<Token> scan(String input) {
        source = input;  current = 0;
        List<Token> tokens = new ArrayList<>();

        while (!isAtEnd()) {
            while (skipIrrelevant()) {}
            if (!isAtEnd()) tokens.add(nextToken());
        }
        tokens.add(new Token(EOF, "<EOF>"));
        return tokens;
    }

    protected abstract Token nextToken();

    // Hilfsmethoden: isAtEnd(), peek(), advance() ...
}
```

Der Lexer läuft über den Eingabestring und erzeugt eine vollständige Liste mit Token. Irrelevante Zeichen wie Whitespace und Kommentare werden immer wieder entfernt.

Die Einsprungsfunktion `scan()` stützt sich dabei auf der zentralen Hilfsmethode `nextToken()` ab, die das nächste Token an der aktuellen Eingabeposition bildet. Die Implementierung ist algorithmisch einfach: Wir schauen uns das aktuelle Zeichen an, entscheiden, um welchen Token-Typ es sich handelt, und rücken den Positionszeiger entsprechend vor. Falls nötig, ziehen wir weitere Zeichen in diese Entscheidung mit ein.

*Anmerkung*: In Lispy werden Whitespaces und Kommentare in späteren Verarbeitungsstufen nicht mehr benötigt und deshalb entfernt. In Sprachen wie Python spielen Whitespaces aber eine wichtige syntaktische Rolle und werden deshalb geeignet als Token erfasst und an den Parser weitergegeben.

> [!TIP]
>
> Der gezeigte Aufbau wirkt zunächst unnötig komplex, insbesondere die `while`-Schleife in `scan()`. Dies liegt u.a. daran, dass wir für die nächsten Sprachstufen jeweils eigene Klassen anlegen, die von den Klassen der Vorgängerstufe erben. Für den Lexer wäre dies beispielsweise `class LispyLexerLexpr extends LispyLexerLint` usw. Dabei wird das neue Verhalten durch Überschreiben der entsprechenden Methoden erreicht und für das bisherige Verhalten nur `super.xyz()` aufgerufen. Das orientiert sich am Vorgehen in ([Siek 2023](#ref-Siek2023python)) und ist relativ elegant, kommt aber mit dem Preis, dass man bei Zustandsveränderungen gut aufpassen muss - wenn die überschreibende Methode ein `advance()` macht und dann ihre Basisimplementierung aufruft, dann fehlt dort das bereits gelesene Zeichen ...

> [!TIP]
>
> ### Design-Diskussion: Schnittstelle Lexer
>
> In der gezeigten Variante verarbeitet der Lexer den kompletten Input und erzeugt (falls möglich) eine vollständige Liste mit den zur Eingabe gehörenden Token. Der nachfolgende Parser kann damit vom Lexer entkoppelt werden und bekommt als Input einfach eine Liste mit Token und arbeitet auf dieser Liste - die Parserimplementierung muss nicht wissen, dass es eine Klasse `Lexer` gibt und wie diese aussieht. Der Preis ist dabei möglicher Mehrbedarf an Speicher und möglicherweise schlechtere Fehlermeldungen.

## Beispielimplementierung Lexer für L-Int

``` java
public class LispyLexerLint extends BaseLexer {

  protected Token nextToken() {
    return switch (peek()) {
      case '(' -> new Token(LPAREN, advance());
      case '+' -> new Token(PLUS, advance());
      default -> scanLiteral(); // Integer Literals
    };
  }

  protected Token scanLiteral() {
    if (Character.isDigit(peek())) {
      String lexeme = readWhile(Character::isDigit);
      return new Token(INT, lexeme);
    }

    throw new RuntimeException("unexpected character: '" + peek() + "'");
  }
}
```

Hier ist eine mögliche Implementierung von `nextToken()`. In der aufrufenden Methode `scan()` hatten wir Whitespaces und Kommentare übersprungen und geprüft, ob wir am Ende der Eingabe sind. Wir können hier also unbesorgt das aktuelle Zeichen lesen und entscheiden, welches Token hier vorliegt (oder beginnt). Gelesene Zeichen werden aus der Eingabe "entfernt": Beim Aufruf von `advance()` wird der Index `current` inkrementiert (falls möglich).

Wenn das aktuelle Zeichen eine Ziffer ist, lesen wir so lange weiter, bis keine Ziffer mehr folgt, und erzeugen ein `INT`-Token. Andernfalls unterscheiden wir anhand des Zeichens die Operatoren und Klammern. Unbekannte Zeichen führen zu einem Fehler. Das ist im Kern das, was auch generierte Lexer tun.

**Wichtig**: Die Subtraktion fehlt hier und dient als kleine Übung.

*Anmerkung*: Häufig findet man im Lexer keinen "schönen" objektorientierten Ansatz. Das hat häufig Performancegründe ...

## Hilfsfunktionen im Lexer I: Zeichenhandling

``` java
public abstract class BaseLexer {

    protected final boolean isAtEnd() { return current >= source.length(); }

    protected final char peek() { return peek(0); }
    protected final char peek(int la) {
        if (la < 0 || current + la >= source.length()) return '\0';
        return source.charAt(current + la);
    }

    protected final char advance() {
        if (isAtEnd()) return '\0';
        return source.charAt(current++);
    }
}
```

Wir müssen im Lexer immer wieder erkennen, ob bereits alle Zeichen im Input abgearbeitet sind. Um nicht immer wieder den Index `current` mit der Länge des Inputs `source` vergleichen zu müssen, kapselt man dies in der Hilfsmethode `isAtEnd()`. Diese liefert `true`, sobald wir alle Zeichen im Input-String `source` bearbeitet haben.

In der Hauptschleife `nextToken()` müssen wir auf das aktuelle Zeichen schauen. Auch hier bietet es sich an, dies nicht immer wieder manuell über `source.charAt(current)` zu tun, sondern dies über eine Hilfsmethode `peek()` zu tun. Im obigen Beispiel ist auch gleich eine Implementierung für ein `peek()` mit Lookahead gezeigt - wir werden später auch das nächste und übernächste Zeichen nach dem aktuellen Zeichen benötigen.

Für das Weiterschalten gibt es die Methode `advance()`. Diese gibt das aktuelle Zeichen zurück und schaltet auf das nächste Zeichen, d.h. ein nachfolgendes `peek()` liefert dann das nächste Zeichen. Damit wir nicht über den Input-String hinaus laufen, wird die Weiterschaltung nur gemacht, wenn wir noch nicht am Ende des Inputs sind. Anderenfalls liefern wir ein Null-Zeichen (`\0`) zurück.

*Anmerkung*: Der gezeigte Lexer verändert den Eingabestring nicht: Er verschiebt lediglich den Index `current`. "Zeichen werden aus der Eingabe entfernt" bedeutet hier also, dass der Index `current` inkrementiert wird.

> [!TIP]
>
> ### Design-Diskussion: Index-Handling
>
> Das Hantieren mit dem Index und dem Weiterschalten muss gut aufeinander abgestimmt sein - der Lexer sollte hier nicht in einen ungültigen Zustand geraten können.
>
> Grundsätzlich könnte man es den Usern überlassen, die Aufrufe immer korrekt zu machen und damit nie über das Ende des Inputs hinaus zu lesen. In der Praxis lohnt es sich aber, etwas mehr Robustheit einzubauen.
>
> Im obigen Beispiel habe ich die Rückgabe eines besonderen Null-Zeichens gewählt, dies stammt aus der C-Welt und bedeutet dort "wir sind am Ende des Strings angekommen".
>
> Alternativ könnte man auch eine Exception auslösen beim Versuch, am Ende des Strings noch ein `advance()` zu machen. Dies zieht aber schnell eine relativ umständliche Fehlerbehandlung nach sich, zumal wir unsere Lexer für die Sprachstufen schrittweise aufbauen und voneinander ableiten werden.

## Read-Ahead: Unterscheiden von "=" und "=="

``` java
protected Token nextToken() {
    return switch (peek()) {

        case '=' -> {
            advance();  // consume first '='
            if (peek() == '=') {
                advance();  // consume second '='
                yield new Token(EQEQ, "==");
            }
            else yield new Token(EQUAL, "=");
        }

        default -> super.nextToken();
    };
}
```

Um die Token "`=`" und "`==`" unterscheiden zu können, müssen wir ein Zeichen vorausschauen: Wenn nach dem ersten "`=`" noch ein zweites "`=`" kommt, ist es "`==`", sonst "`=`".

Erinnerung: Die Funktion `advance()` liefert das aktuelle Zeichen aus der Eingabe zurück und schaltet zum nächsten Zeichen.

**Hinweis**: In der `if`-Abfrage im `case '='` könnte man auch `peek(1)` einsetzen, dann müsste man nur die Reihenfolge der `advance()` anpassen.

## Hilfsfunktionen im Lexer II: Lese solange ...

``` java
public abstract class BaseLexer {

    protected final boolean skipIrrelevant() {}  // Homework

    protected final String readWhile(Predicate<Character> pred) {
        StringBuilder sb = new StringBuilder();
        while (!isAtEnd() && pred.test(peek())) sb.append(advance());
        return sb.toString();
    }
}
```

Wir brauchen noch zwei weitere komplexere Hilfsfunktionen im Lexer: `skipIrrelevant()` und `readWhile()`.

Der Aufruf von `skipIrrelevant()` "entfernt" vom aktuellen Zeichen an alle nicht benötigten Zeichen wie Whitespace und Kommentare und stoppt, sobald ein benötigtes Zeichen auftaucht. Dabei muss die Funktion selbst prüfen, ob überhaupt noch gelesen werden darf, oder ob bereits das Ende des Inputs erreicht ist. Falls Zeichen entfernt wurden, wird `true` zurückgegeben. (In unserer Implementierung springt der Index `current` weiter, es werden keine Zeichen physisch entfernt.)

Die Funktion `readWhile()` bekommt einen Test `pred` als Argument und liest so lange, wie das aktuelle Zeichen diesen Test erfüllt und das Ende der Eingabe nicht erreicht ist. Jedes gelesene Zeichen wird an einen StringBuilder angehängt und am Ende als String zurückgeliefert. Diese Funktion ist sehr praktisch, wenn man beispielsweise aufeinander folgende Ziffern einlesen will: `String lexeme = readWhile(Character::isDigit);` liest so lange, wie Ziffern in der Eingabe vorkommen und liefert diese zunächst als String zurück.

## Typische Muster für Erstellung von Token

1.  Schlüsselwörter

    -   Ein eigenes Token für jedes Schlüsselwort, oder
    -   Erkennung als Name (`ID`) und nachträglich Vergleich mit Wörterbuch sowie Korrektur des Tokentyps

    Wenn Schlüsselwörter über je ein eigenes Token abgebildet werden, benötigt man für jedes Schlüsselwort einen eigenen RE bzw. DFA. Die Erkennung als Bezeichner und das Nachschlagen in einem Wörterbuch (geeignete Hashtabelle) sowie die entsprechende nachträgliche Korrektur des Tokentyps kann die Anzahl der Zustände im Lexer signifikant reduzieren! Wir werden dies später in der Sprachvariante L-Var sehen.

2.  Operatoren

    -   Ein eigenes Token für jeden Operator, oder
    -   Gemeinsames Token für jede Operatoren-Klasse

3.  Bezeichner: Ein gemeinsames Token für alle Namen

4.  Zahlen: Ein gemeinsames Token für alle numerischen Konstanten (ggf. Integer und Float unterscheiden)

    Für Zahlen führt man oft ein Token "`NUM`" oder "`INT`"ein. Als Attribut speichert man das Lexem als String. Alternativ kann man das Lexem bereits in eine Zahl konvertieren und als (zusätzliches) Attribut speichern. Dies kann in späteren Stufen viel Arbeit sparen.

5.  String-Literale: Ein gemeinsames Token

6.  Komma, Semikolon, Klammern, ...: Je ein eigenes Token

7.  Regeln für Whitespace und Kommentare etc. ...

    Normalerweise benötigt man Kommentare und Whitespaces in den folgenden Stufen nicht und entfernt diese deshalb aus dem Eingabestrom. Dabei könnte man etwa Whitespaces in den Pattern der restlichen Token berücksichtigen, was die Pattern aber sehr komplex macht. Die Alternative sind zusätzliche Pattern, die auf die Whitespace und anderen nicht benötigten Inhalt matchen und diesen "geräuschlos" entfernen. Mit diesen Pattern werden keine Token erzeugt, d.h. der Parser und die anderen Stufen bemerken nichts von diesem Inhalt.

    Gelegentlich benötigt man aber auch Informationen über Whitespaces, beispielsweise in Python. Dann müssen diese Token wie normale Token an den Parser weitergereicht werden.

Jedes Token hat üblicherweise ein Attribut, in dem das Lexem gespeichert wird. Bei eindeutigen Token (etwa bei eigenen Token je Schlüsselwort oder bei den Interpunktions-Token) kann man sich das Attribut auch sparen, da das Lexem durch die Tokenart eindeutig rekonstruierbar ist. Bei unserer Implementierung in Java müsste man dann entweder zwei verschiedene Klassen zur Modellierung der Token nutzen (was eher umständlich ist), oder man spendiert der `Token`-Klasse Hilfsfunktionen, die ein Token bauen: einmal vollständig (mit Token-Typ und Lexem) und einmal nur mit dem Token-Typ:

``` java
public record Token(TokenKind kind, String lexeme) {

  public static Token from(TokenKind kind) {
    return from(kind, kind.toString());
  }

  public static Token from(TokenKind kind, char lexeme) {
    return from(kind, String.valueOf(lexeme));
  }

  public static Token from(TokenKind kind, String lexeme) {
    return new Token(kind, lexeme);
  }

}
```

> [!TIP]
>
> ### Anmerkung: Längster Match
>
> Wenn es mehrere matchende Tokenregeln gibt, wird in der Regel das längste Lexem bevorzugt.
>
> Wenn es mehrere gleich lange Alternativen gibt, muss man mit Vorrangregeln bzgl. der Token arbeiten. Bei ANTLR wäre das die Reihenfolge, in der die Token definiert wurden.
>
> Bei einem handgeschriebenen Lexer muss die Implementierung dieses Verhalten selbst festlegen.

## Fehler bei der lexikalischen Analyse

Problem: Eingabestrom sieht so aus: `(fi (== a 42) ...)`

Der Lexer kann nicht erkennen, ob es sich bei `fi` um ein vertipptes Schlüsselwort handelt oder um einen Bezeichner: Es könnte sich um einen Funktionsaufruf der Funktion `fi()` handeln ... Dieses Problem kann erst in der nächsten Stufe (Parser) sinnvoll erkannt und behoben werden.

=\> Was tun, wenn keines der Pattern auf den Anfang des Eingabestroms passt?

Optionen:

-   Aufgeben ...

    Eventuell vielleicht sogar die beste und einfachste Variante :-)

<!-- -->

-   "Panic Mode": Entferne so lange Zeichen, bis ein Pattern passt.

    Das verwirrt u.U. den Parser, kann aber insbesondere in interaktiven Umgebungen hilfreich sein. Ggf. kann man dem Parser auch signalisieren, dass hier ein Problem vorlag.

<!-- -->

-   Ein-Schritt-Transformationen:
    -   Füge fehlendes Zeichen in Eingabestrom ein.
    -   Entferne ein Zeichen aus Eingabestrom.
    -   Vertausche ein Zeichen:
        -   Ersetze ein Zeichen durch ein anderes.
        -   Vertausche zwei benachbarte Zeichen.

    Diese Transformationen versuchen, den Input in einem Schritt zu reparieren. Das ist durchaus sinnvoll, da in der Praxis die meisten Fehler in dieser Stufe durch ein einzelnes Zeichen hervorgerufen werden: Es fehlt ein Zeichen oder es ist eines zu viel im Input. Es liegt ein falsches Zeichen vor (Tippfehler) oder zwei benachbarte Zeichen wurden verdreht ...

    Allerdings muss man sich in Erinnerung rufen, dass gerade in dieser ersten Phase eines Compilers die Geschwindigkeit stark im Fokus steht und eine ausgefeilte Fehlerkorrekturstrategie die vielen kleinen Optimierungen schnell wieder zunichte machen kann. Obendrein kann der Lexer ohne weiteren syntaktischen oder semantischen Kontext oft nicht zuverlässig entscheiden, was gemeint war.

<!-- -->

-   Fehler-Regeln: Matche typische Typos

    Gelegentlich findet man in den Grammatiken für den Lexer extra Regeln, die häufige bzw. typische Typos matchen und dann passend darauf reagieren.

## Wrap-up

-   Lexer zerlegt den Quelltext von links nach rechts in Token
    -   Token: hat Tokenart und Lexem
    -   Reguläre Ausdrücke beschreiben die Formen der Token
    -   Whitespace und Kommentare überspringen (sofern nicht für spätere Verarbeitungsstufen benötigt)

<!-- -->

-   Implementierungsansatz: Zeichenweise Iteration durch die Eingabe
    -   Entscheidung für ein Token am aktuellen Zeichen (plus ggf. weitere folgende Zeichen ("Lookahead"))
    -   Ggf. Lookahead notwendig, z.B. bei "==" vs. "="
    -   Bevorzugung des längsten Matches
    -   Ende der Eingabe: Künstliches `EOF`-Token

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Zum Thema Lexing findet sich eine schöne Darstellung in ([Nystrom 2021](#ref-Nystrom2021)) in Kapitel 4 "Scanning".
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k1: Ich kenne die Aufgaben eines Lexers
> -   k2: Ich kann typische Fehler und Lösungsansätze in der lexikalischen Analyse erklären
> -   k3: Ich kann für ein Problem eine typische Einteilung von Token vornehmen
> -   k3: Ich kann einen Top-Down-Lexer mit Read-Ahead implementieren
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Design-Diskussion**:
>
> 1.  Modellierung Token
>
>     Welche Möglichkeiten, Token zu modellieren, gibt es noch? Welche Vor- und Nachteile haben die Varianten?
>
> 2.  Schnittstelle Lexer
>
>     Eine häufige Variante der Lexer-Schnittstelle ist die Methode `nextToken()` als Einsprungmethode in den Lexer: `public Token nextToken()`. Der Parser bekommt dann ein Lexer-Objekt und ruft selbst immer wieder bei Bedarf `nextToken()` auf. Was könnten Vor- und Nachteile zur oben im Skript gezeigten Variante sein?
>
> **Token-Strom vorhersagen: Wo endet das Lexem?**
>
> Bestimmen Sie für jede Eingabe den Token-Strom, den der Lexer erzeugen sollte. Geben Sie jeweils Tokenart und Lexem an; das synthetische EOF-Token können Sie dazuschreiben.
>
> ``` clojure
> (+ 12 345)
> (- 7 8)
> (+12 3)
> (+ 12a 3)
> ```
>
> Diskussionsfragen:
>
> -   Gehört ein Vorzeichen zum Integer-Literal oder ist es ein eigenes Token? Begründen Sie Ihre Entscheidung anhand der Tokenregeln für L-Int.
> -   Was passiert bei der letzten Eingabe: Erkennt der Lexer "12a" als ein fehlerhaftes Integer-Literal, oder liest er zunächst "12" als INT?
>
> **nextToken() für L-Int vervollständigen**
>
> Die Beispielimplementierung erkennt `(`, `+` und Integer-Literale. Ergänzen Sie sie so, dass sie auch `-` und `)` als Token erkennt.
>
> Skizzieren Sie Ihren Code. Prüfen Sie anschließend gemeinsam, ob Ihr Lexer bei dieser Eingabe alle Zeichen genau einmal verarbeitet:
>
> ``` clojure
> (- 12 (+ 3 45))
> ```
>
> Diskussionsfragen:
>
> -   Welche Aufgabe übernimmt `advance()` in den einzelnen Fällen?
> -   Warum wird bei einem Integer-Literal nicht nur ein Zeichen gelesen?
>
> **Fehlerfall mit einer klaren Regel behandeln**
>
> Der gezeigte Lexer bricht bei einem unbekannten Zeichen mit einer Exception ab. Überlegen Sie sich für L-Int eine einfache alternative Strategie: Der Lexer soll beim ersten unbekannten Zeichen eine verständliche Fehlermeldung erzeugen und anschließend entweder abbrechen oder weiter nach dem nächsten erkennbaren Token suchen.
>
> Verwenden Sie beispielsweise diese Eingabe:
>
> ``` clojure
> (+ 12 @ 3)
> ```
>
> Legen Sie fest, wie sich der Lexer verhalten soll, und beschreiben Sie die resultierenden Token beziehungsweise die Fehlermeldung.
>
> **Wichtig**: Falls Ihr Lexer nach einem Fehler weiterläuft: Formulieren Sie eine Regel, die sicherstellt, dass er dabei nicht an derselben Eingabeposition hängen bleibt.
>
> </details>

------------------------------------------------------------------------

> [!NOTE]
>
> <details >
> <summary><strong>👀 Quellen</strong></summary>
>
> <div id="refs" class="references csl-bib-body hanging-indent">
>
> <div id="ref-Nystrom2021" class="csl-entry">
>
> Nystrom, R. 2021. *Crafting Interpreters*. Genever Benning. <https://github.com/munificent/craftinginterpreters>.
>
> </div>
>
> <div id="ref-Siek2023python" class="csl-entry">
>
> Siek, J. G. 2023. *Essentials of Compilation: An Incremental Approach in Python*. The MIT Press. <https://github.com/IUCompilerCourse/Essentials-of-Compilation>.
>
> </div>
>
> </div>
>
> </details>

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> 91604b3 2026-10-11 lint: final format for beamer<br></sub></sup></p></blockquote>
