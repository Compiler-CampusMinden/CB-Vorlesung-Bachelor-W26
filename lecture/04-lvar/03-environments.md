# L-Var: Interpreter mit Environments und Statements

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🎯 TL;DR</strong></summary>
>
> In L-Var erweitern wir den Interpreter um Anweisungen (Statements) und modellieren dazu den Laufzeitzustand mit sogenannten Environments. Ein solches `Env` steht für einen lexikalischen Scope zur Laufzeit und enthält eine Zuordnung von Namen zu Werten sowie einen Zeiger auf das Eltern-Environment. Beim Betreten eines neuen Blocks (`BlockStmt`) erzeugen wir ein neues `Env` mit dem bisherigen Environment als Eltern, beim Verlassen setzen wir `env` wieder auf das vorherige Environment zurück.
>
> Variablen werden über `VarDecl` im aktuellen Environment definiert: Wir werten die Initialisierungsausdrücke aus und speichern den resultierenden Wert unter dem Variablennamen im aktuellen Environment. Lesezugriffe auf Variablen benutzen nicht mehr eine rekursive Suche durch die Environment-Struktur nach "oben", sondern greifen mithilfe der im Name-Resolver vorberechneten Tiefe gezielt auf das passende Environment zu. Der Name-Resolver hat bereits geprüft, ob Namen definiert und sichtbar sind und ob es keine Mehrfachdefinitionen gibt, sodass der Interpreter hier keine aufwändige Fehlerprüfung mehr benötigt.
>
> Expressions werten wir weiterhin mit `eval(expr)` aus, Statements mit einer neuen Methode `exec(stmt)`. Ein Programm in L-Var ist eine Liste von Statements; beim Interpretieren durchlaufen wir alle Statements und merken uns den Wert der jeweils letzten ausgewerteten Expression (`ExprStmt`). Damit trennen wir klar zwischen statischer Namensauflösung mit `Scope` und dynamischer Ausführung mit `Env`, obwohl beide Strukturen äußerlich ähnlich aussehen.
>
> </details>

## Erinnerung: Interpreter für L-Expr

``` java
public class InterpreterLexpr extends InterpreterLint {

    @Override
    public Value eval(Expr expr) {
        return switch (expr) {
            case MulExpr(var l, var r) -> {
                int a = asInt(eval(l));
                int b = asInt(eval(r));
                yield new IntVal(a * b);
            }

            default -> super.eval(expr);
        };
    }
}
```

Wir hatten eine Methode `Value eval(Expr expr)` zum Auswerten von Ausdrücken (Expressions) definiert. Da der `InterpreterLexpr` von `InterpreterLint` ableitet, überschreiben wir diese Methode. Wir müssen nur die neuen Auswertungen ergänzen, und für die bisherigen Fälle rufen wir einfach `super.eval(expr)` auf.

Diese werden wir in L-Var wie gewohnt mit weiteren Ausdrücken ergänzen.

Aber wir haben in L-Var zusätzlich noch Anweisungen (Statements), die bei der Auswertung keinen Wert liefern. Die Statements können wir ähnlich auswerten wie die Expressions, aber wir brauchen noch ein Konzept, wie wir mit dem Zustand umgehen ...

## Zustände: Auswerten von Anweisungen

``` clojure
(def x : int 1)
(def a : int 2)
(let ((b : int 3)
      (x : int 42))
  (+ a x))
```

    +--------------------+
    |     GLOBAL ENV     |
    +--------------------+
    | values = [x : 1,   |
    |           a : 2]   |
    +--------------------+
               ^
               |
               |
    +--------------------+
    |     LOCAL ENV      |
    +--------------------+
    | values = [b : 3,   |
    |           x : 42]  |
    +--------------------+

Das erinnert nicht nur zufällig an den Aufbau der Symboltabellen ...

Und so lange es nur um Variablen ginge, könnte man die Symboltabellen (`Scope`) erweitern und für das Speichern der Werte nutzen. Allerdings müssen wir noch Funktionen und Klassen realisieren, und spätestens dann funktioniert diese Idee nicht mehr! (vgl. Challenge)

Also lohnt es sich, eigenständige Strukturen für das Halten von Variablen und Werten aufzubauen: "Environments".

## Environments - Laufzeit-Sicht auf Scopes

Wir benötigen analog zu `Scope` eine Datenstruktur, die einen Verweis auf das Eltern-Environment hat und die zu einem Namen Werte (also `Value` statt `Stmt` wie in `Scope`) speichert. Nachfolgend die Kernfunktionalität (unvollständig):

``` java
public record Env(Env parent, Map<String, Value> values) {

    public void define(String name, Value value) { values.put(name, value); }

    public Value get(String name, int depth) {
        Env env = envAtDepth(depth);
        return env.values.get(name);
    }

    private Env envAtDepth(int depth) {
        return (depth == 0) ? this : parent.envAtDepth(depth - 1);
    }
}
```

-   `Env` bildet **lexikalische Scopes zur *Laufzeit*** ab:
    -   `parent` zeigt auf das "umgebende" Environment ("Eltern-Environment")
    -   `values` speichert zu einem Namen den Wert für den aktuellen Scope
-   Zugriffsmöglichkeiten:
    -   Definieren von Namen mit `define()`
    -   Abfrage von Werten mit `get()`

Hier modellieren wir die Semantik von Lispy mit. Ein Name (Variable, ...) kann nur genau einmal pro Scope definiert werden - Mehrfachdefinitionen stellen einen Fehler dar. Das haben wir bereits in einem früheren Pass (Name-Resolver) geprüft, deshalb können wir hier das Definieren eines Namens in `define()` einfach halten. Das Definieren eines Namens (hier einer Variablen) passiert immer im aktuellen Scope/Environment.

Für den Abruf eines Wertes zu einem Namen mit `get()` nutzen wir die im früheren Pass (Name-Resolver) berechnete Tiefeninformation: Dort hatten wir bereits für jeden Namen (a) geprüft, ob dieser vom aktuellen Scope aus sichtbar ist, und (b) berechnet, wie viele Schritte (Scopes) man nach "oben" gehen muss. Eine `depth = 0` bedeutet dabei das aktuelle Environment, `depth = 1` das direkte Eltern-Environment usw. - genau mit dieser Konvention hat der Name-Resolver die Tiefen berechnet. Diese Information nutzen wir jetzt: Wir suchen uns nicht mehr rekursiv durch die Environments nach "oben" (wie in `Scope#resolve()`), sondern lassen uns gezielt das passende (Eltern-) Environment geben und rufen dort in der Hashmap `values` den gespeicherten Wert ab. Dass der Name vorhanden ist und in dieser "Tiefe", hatten wir bereits im Name-Resolver geprüft und müssen das hier nicht wiederholen.

Wir haben eine *immutable* Semantik in Lispy, d.h. Variablen können im Laufe ihres Lebens **keine** neuen Werte zugewiesen werden. Deshalb müssen wir in `Env` keine Möglichkeit zum Ändern bereits gebundener Werte implementieren.

> [!TIP]
>
> ### (Mini-) C++ kennt Zuweisungen
>
> Wenn es wie in (Mini-) C++ Zuweisungen in der Sprache gibt, dann braucht man eine Möglichkeit, um Namen in einem `Env` einen neuen Wert zuzuweisen. Für diese Neuzuweisung von Namen kann man sich eine Methode `void assign(String name, Value value, int depth)` in der Klasse `Env` definieren, die analog zu `get()` das Elternenvironment in der Tiefe `depth` aufsucht und dort dem Namen `name` den neuen Wert `value` zuweist. (siehe Challenge/Hausaufgabe)

> [!IMPORTANT]
>
> ### Zusammenspiel semantische Analyse und Interpretation
>
> Die Prüfung der aufgezählten Fehlerzustände ist bereits im Name-Resolver (`NameResolverLvar` in Verbindung mit `Scope`) erfolgt! Dort haben wir bereits geprüft:
>
> -   ob ein Name doppelt definiert (gebunden) werden soll: bei der Behandlung von `VarDecl` mit `Scope#bind()`
> -   ob ein Name vom aktuellen Scope/Environment aus sichtbar ist: bei der Behandlung von `VarExpr` mit `Scope#resolve()`
>
> Für jedes `define()` und `get()` hier im Interpreter bzw. Environment haben wir (wenn wir alles richtig gemacht haben) bereits **vorher** im Name-Resolver und Symboltabelle ein entsprechendes `bind()` und `resolve()` geprüft.
>
> **Deshalb braucht der Code für `Env` für diese Punkte keine extra Fehlerprüfungen!**
>
> Wer sich damit unwohl fühlt, kann gern hier zusätzliche Prüfungen mit einbauen. Dann bitte aber in der Fehlermeldung auf einen **internen Fehler** hinweisen - diese Inkonsistenzen hätten nämlich bereits in der semantischen Analyse auffallen müssen!

> [!TIP]
>
> ### Implementierungsdetail `Env`
>
> Es lohnt sich, der Klasse `Env` wieder zwei extra Hilfskonstruktoren mitzugeben:
>
> ``` java
> public record Env(Env parent, Map<String, Value> values) {
>
>     public Env() { this(null); }
>     public Env(Env parent) { this(parent, new HashMap<>()); }
>
>     // ...
> }
> ```
>
> Das erleichtert später das Anlegen neuer Environments.

> [!TIP]
>
> ### Exkurs: Nested Environments vs. Stack von Environments
>
> Unsere Environments sind analog zu den Scopes aufgebaut und haben einen Verweis auf das Eltern-Environment. Damit kann man einen Namen rekursiv vom aktuellen Environment aus nach "oben" bis zur Wurzel (globales Environment) suchen.
>
> In vielen Fällen könnte man auf diese Verzeigerung verzichten und im Interpreter einfach einen Stack von einfachen Environments pflegen - jeder neue Scope legt ein neues Environment oben auf den Stack, jedes Verlassen eines Scopes entfernt den obersten Eintrag. Das funktioniert sogar für Funktionsaufrufe!
>
> Es gibt aber Szenarien, wo dies nicht ausreicht. Wir werden möglicherweise für unser Lispy so ein Szenario sehen. Können Sie sich selbst ein Szenario überlegen, wo ein `Stack<Env>` im Interpreter nicht reicht und wir wissen müssen, was das Eltern-Environment `Env parent` ist? (vgl. Challenges)

> [!IMPORTANT]
>
> ### Unterscheidung Symboltabellen (`Scope`) und Environments (`Env`)
>
> `Scope` beantwortet "Wo ist ein Name definiert?", `Env` beantwortet "Welchen Wert hat dieser Name gerade in diesem Aufrufkontext?".
>
> Beide Strukturen (`Scope` für die Symboltabellen und `Env` für die Environments) sind strukturell ähnlich aufgebaut. Beide haben einen Verweis auf den Eltern-Scope bzw. das Eltern-Environment. Beide speichern zu einem Namen eine Information. Beide haben eine Art Auflösemechanismus, um die Information zu einem Namen abzurufen. Beide dienen ähnlichen Zwecken: Ein `Scope`-Objekt spiegelt wie ein `Env`-Objekt einen Sichtbarkeitsbereich (Scope) in unserem zu verarbeitenden Programm.
>
> Der Unterschied liegt in der Benutzung:
>
> -   `Scope` ist eine **statische** Sicht auf die Sichtbarkeitsbereiche, d.h. sie hängt **nur von der Quelltextstruktur** ab, nicht vom tatsächlichen Programmverlauf.
>
>     In einem `Scope`-Objekt ordnen wir während der semantischen Analyse jedem im Scope sichtbaren Namen genau seinen Deklarationsknoten (`Stmt`) im AST zu. Wir bauen mit `Scope` die Symboltabellenstruktur im Name-Resolver auf. Dabei betreten wir jeden Scope im Quelltext (AST), beispielsweise das `let`-with-Scopes (oder später in L-Fun den Funktionskörper) und prüfen, ob wir semantischen Regeln der Sprache Lispy eingehalten wurden: Sind alle Namen sichtbar (ggf. über die Eltern-Beziehung bis zum globalen Scope hoch), werden keine Namen im selben Scope mehrfach definiert, ist der Name bei einem Lesevorgang bereits vorher definiert worden? Entsprechend speichern wir zu einem Namen die Information der Definition, also den `Stmt`-Knoten des AST, der zu diesem Namen gehört. Wir werden später in L-Fun sehen, dass wir zwar die lokalen Variablen im Funktionskörper ganz normal auflösen, aber keine Funktionsaufrufe verfolgen! **Wir interpretieren in der semantischen Analyse keine Anweisungen oder Ausdrücke, sondern prüfen nur die Sichtbarkeiten.**
>
> -   `Env` ist die **dynamische** Sicht auf die Sichtbarkeitsbereiche.
>
>     Wir speichern bei der Interpretation zu jedem Namen den zugehörigen Wert in der `Env`. **Wir interpretieren nun die Ausdrücke und Anweisungen**, d.h. wir berechnen Werte und erzeugen Nebeneffekte. Jedes Env-Objekt entspricht einem **konkreten Aktivierungskontext eines Scopes** im Programm (z.B. einem Funktionsaufruf). Derselbe Funktionskörper kann also zur Laufzeit viele verschiedene `Env`-Objekte haben - eine `Env` pro Aufruf. Die Environment-Struktur baut sich dynamisch während der Verarbeitung auf und ab, und im Unterschied zur semantischen Analyse verfolgen wir später in L-Fun dann (natürlich) auch die Funktionsaufrufe - jeder Funktionsaufruf legt einen neuen "Scope" an, in dem die lokalen Variablen im Funktionskörper für diesen Aufruf definiert sind. Dies nennt man auch oft "Call Frame" oder "Stack Frame" - unsere `Env`-Objekte modellieren genau diese Call Frames. In vielen Implementierungen wird dafür intern tatsächlich ein Stack von Frames verwendet, aber manche Dinge gehen leider nur über die Struktur mit dem Eltern-Zeiger - siehe Challenges. Unsere Eltern-Zeiger-Struktur bildet dasselbe Konzept explizit im Heap nach. Wir führen hier im Gegensatz zur Namensauflösung alle Schleifen und Funktionsaufrufe im verarbeiteten Programm aus und erzeugen dabei die gewünschten Seiteneffekte. Wir haben jetzt Kontrollfluss und interpretieren beispielsweise Fallunterscheidungen - je nachdem, wie die Auswertung der Bedingung ausfällt, betrachten wir jetzt nur noch einen der Branches ...
>
> **Merksatz**:
>
> -   `Scope` (semantische Analyse/Namensauflösung):
>     -   Name $\to$ Deklaration (`*Decl`-Knoten im AST)
>     -   statisch, ein oder mehrere Passes über den AST
>     -   kein Kontrollfluss, keine Ausführung
> -   `Env` (Interpreter):
>     -   Name $\to$ Wert zur Laufzeit
>     -   dynamisch, verändert sich während der Ausführung
>     -   führt Kontrollfluss, Schleifen, Funktionsaufrufe tatsächlich aus

## Interpreter für L-Var - Überblick

### Vorüberlegungen

Der Interpreter für L-Var wird wie üblich vom Interpreter der Vorstufe ableiten, d.h. wir haben `class InterpreterLvar extends InterpreterLexpr`.

In L-Var haben wir eine zusätzliche Expression: `VarExpr`. Wir überschreiben entsprechend die geerbte Methode `eval(expr)` und fügen einen neuen Fall hinzu.

Zusätzlich haben wir in L-Var jetzt Statements. Da Statements "ausgeführt" werden und dabei Nebeneffekte erzeugen, werden wir analog zu den Expressions eine Methode `exec(stmt)` einführen, um die Statements zu verarbeiten.

Außerdem besteht ein L-Var-Programm aus einer Liste von Statements: `Lvar = [Stmt]`. Wir benötigen entsprechend eine neue Einstiegsmethode und Felder, um den Zustand zu speichern. Wir bekommen zusätzlich aus der semantischen Analyse die Symboltabellen und die Sprunginformationen (gekapselt in einen Sidetable).

Wenn unser Interpreter nun eine Liste von Statements abarbeitet - was soll er dann zurückliefern? Eine Expression evaluiert ja zu einem Wert, aber Statements nicht. Trotzdem wäre es schön, wenn wir den zuletzt erzeugten Wert speichern und zurückgeben könnten: Wir haben ja das `ExprStmt`, welches eine Expression ist - den Wert dieser Expression könnten wir zurückgeben, so in unserem Programm überhaupt ein `ExprStmt` vorkommt. Wenn es mehrere davon gibt, interessiert uns das zuletzt ausgewertete `ExprStmt`.

### Start des Interpreters

``` java
public class InterpreterLvar extends InterpreterLexpr {
    protected SemaInfo sema;
    protected Env env;  // current environment
    protected Value last;

    public Value interpret(List<Stmt> stmts, SemaInfo semaInfo) {
        sema = semaInfo;  env = sema.globalEnv;  last = null;

        stmts.forEach(this::exec);

        return last;
    }

    // protected void exec(Stmt stmt) ...
    // protected Value eval(Expr expr) ...
}
```

Damit haben wir den Kern des Interpreters für L-Var aufgebaut.

Wir benötigen ein Start-Environment, welches wir über den Sidetable `semaInfo` holen. Dazu ergänzen wir diese Klasse wie folgt:

``` java
public class SemaInfo {
    public Scope globalScope = new Scope();
    public Map<Stmt, Scope> symbols = new IdentityHashMap<>();
    public Map<Expr, Integer> depths = new IdentityHashMap<>();

    public Env globalEnv = new Env();  // NEU
}
```

Da wir das `semaInfo`-Objekt außerhalb des Interpreters anlegen, haben wir so die Gelegenheit, bei Bedarf das globale Environment mit Defaults vorzubefüllen. Das werden wir später in L-Fun tun.

Im Feld `env` merkt sich der Interpreter, in welchem Environment wir uns aktuell befinden. Zu Beginn ist dies das globale Environment. Wann immer wir einen Scope öffnen, erzeugen wir ein neues Kind-Environment (Eltern-Environment ist `env`) und halten es in `env` fest. Wenn wir einen Block verlassen, setzen wir das `env` wieder zurück. Wenn wir alles richtig machen, sollte am Ende der Ausführung des Programms `env` wieder auf dem globalen Environment stehen.

Ansonsten iterieren wir über die Liste der Statements und führen jedes einzelne aus. Die Ergebnisse von `ExprStmt` merken wir uns in `last`, dieses liefern wir am Ende zurück.

## Statements: Ausführen einer Variablendeklaration und eines 'let'-with-Scope

``` ebnf
Stmt = ExprStmt(Expr) | VarDecl(String, Type, Expr?) | BlockStmt([Stmt])
```

Wie oben angeteasert haben wir einige neue Statements in L-Var:

-   `ExprStmt`: "Cast" einer Expression als Statement
-   `VarDecl`: Definiert eine neue Variable (samt potentieller Initialisierung)
-   `BlockStmt`: Öffnet einen neuen lokalen Scope für die Ausführung der enthaltenen Statements

Für diese Statements definieren wir nun die Methode `exec(stmt)` in `InterpreterLvar`:

``` java
protected void exec(Stmt stmt) {
    switch (stmt) {
        case ExprStmt(var expr) -> last = eval(expr);
        case VarDecl(var name, var type, var init) -> {
            Value i = init.map(this::eval).orElseGet(() -> defaultFor(type));
            env.define(name, i);
        }
        case BlockStmt(var s) -> {
            env = new Env(env);
            s.forEach(this::exec);
            env = env.parent();
        }
        default -> throw new RuntimeException("unexpected statement");
    }
}
```

> [!TIP]
>
> **Hinweis**: `exec(stmt)` hat keinen Rückgabewert, ist also `void`! Statements werden ausgeführt, und dabei ergibt sich kein Wert.
>
> Im Unterschied dazu ergeben Expressions bei der Auswertung einen Wert:
>
> ``` java
> void exec(Stmt stmt);
> Value eval(Expr expr);
> ```

### Auswertung von Expression-Statements

In L-Var können keine Expressions mehr direkt in der obersten Sprachebene vorkommen - jede Expression wird in ein `ExprStmt` verpackt.

Um diese auszuwerten, wird einfach die `eval()`-Methode mit der Expression aufgerufen. Dabei entsteht ein Wert, und diesen merken wir uns im Feld `last` des `InterpreterLvar`. Falls wir später noch einmal ein `ExprStmt` verarbeiten, würden wir das Feld entsprechend überschreiben. Damit merken wir uns in diesem Feld den jeweils letzten Wert ...

### Auswertung einer Variablendefinition

Eine Variablendefinition (`VarDecl`) besteht aus drei Teilen: einem Namen, dem Typ und der potentiellen Initialisierung.

Die Initialisierung ist eine Expression - diese werten wir wie gewohnt mit `eval()` aus. Da es sich hier um ein `Optional<Expr>` handelt, müssen wir im Fall des Nichtvorhandenseins einen Defaultwert für den Typ der Variablen bestimmen. Das könnte in L-Var etwa so aussehen:

``` java
protected Value defaultFor(Type t) {
    if (t == BuiltinType.INT) return new IntVal(0);
    throw new RuntimeException("(internal error) unsupported type");
}
```

Als Typ kann in L-Var nur ein `int` vorkommen, mehr erlaubt die Grammatik nicht. Später werden wir noch weitere Typen ergänzen und in einem weiteren AST-Pass vor dem Interpreter prüfen - das ist der sogenannte "Type Checker". Für den Interpreter spielt der Typ an sich aber keine weitere Rolle - wir werten die Initialisierungsexpression aus oder nutzen alternativ einen typabhängigen Defaultwert, und der dabei entstehende Wert passt zum Typ der Variablen, anderenfalls hätte der Type-Checker einen Fehler gemeldet.

> [!TIP]
>
> Die Entscheidung für einen Wert als Defaultwert ist eine wichtige Entscheidung für die Semantik der Sprache! In C/C++ beispielsweise wird in vielen Fällen **keine** automatische Initialisierung vorgenommen, was zwar eine höhere Effizienz zur Laufzeit bewirkt, aber eben auch in meiner Erfahrung eine häufige Quelle für Fehler darstellt.
>
> Wir werden in unserem Mini-C++ deshalb abweichend vom echten C++ **Variablen immer automatisch mit einem Defaultwert initialisieren**, wenn es keinen Initialisierungsausdruck gibt.

Da eine `VarDecl` einen neuen Namen (die Variable) im aktuellen Scope einführt, definieren wir den Namen im aktuellen Environment und speichern den Wert aus der Initialisierungsexpression.

### Auswerten eines `let`

Ein `let`-Statement kapselt Statements, die in einem neuen lokalen Scope ausgeführt werden sollen und bindet ggf. noch lokale Variablen. Im AST finden wir das als `BlockStmt`-Knoten.

Wir legen ein neues Environment an, wobei das bisherige Environment das Eltern-Environment wird, und halten es in `env` fest. Dann iterieren wir über alle Statements im `BlockStmt` und rufen jeweils `exec()` auf. Am Ende setzen wir das Environment wieder auf das alte Environment zurück.

## Expressions: Lese-Zugriff auf Variablen

``` ebnf
Expr = VarExpr(String)
```

In L-Var haben wir eine neue Expression: den lesenden Zugriff auf Variablen, d.h. `VarExpr`. Diese bauen wir wie gewohnt ein, indem wir die `eval()`-Methode überschreiben und passend ergänzen:

``` java
@Override
protected Value eval(Expr expr) {
    return switch (expr) {
        case VarExpr v -> {
            int depth = sema.depths.get(v);
            yield env.get(v.name(), depth);
        }

        default -> super.eval(expr);
    };
}
```

Wenn wir eine Variable lesen wollen, beispielsweise in `(+ a b)`, dann taucht das als `VarExpr` im AST auf. Jede `VarExpr` kapselt dabei den Namen der Variablen (`a`, `b`).

Wir haben vor dem Interpreter den Name-Resolver laufen lassen. Der hat (a) geprüft, ob im aktuellen Scope dieser Name sichtbar ist, und (b) berechnet, wie viele Schritte (Scopes) man nach "oben" gehen muss.

Im Interpreter können wir diese Tiefeninformation aus dem Sidetable abrufen, und dann vom aktuellen Environment entsprechend viele Schritte nach "oben" gehen und dort auf die Variable zugreifen und den dort gespeicherten Wert zurückliefern.

> [!IMPORTANT]
>
> ### Zusammenspiel semantische Analyse und Interpretation
>
> Auch hier gilt: Die notwendigen Prüfungen sind bereits im Name-Resolver (`NameResolverLvar` in Verbindung mit `Scope`) erfolgt! Wenn es dabei einen Fehler gegeben hätte, wären wir nicht im Interpreter angekommen. Beispielsweise wenn die referenzierte Variable nicht sichtbar wäre vom aktuellen Scope aus, dann hätte das der Name-Resolver bereits vorher festgestellt.
>
> **Deshalb brauchen wir hier keine extra Fehlerprüfungen!**
>
> Wer sich damit unwohl fühlt, kann gern hier zusätzliche Prüfungen mit einbauen. Dann bitte aber in der Fehlermeldung auf einen **internen Fehler** hinweisen - diese Fehler hätten nämlich bereits in der semantischen Analyse auffallen müssen!

## Wrap-up

-   Laufzeitzustand: verkettete `Env`-Objekte (Name $\to$ Wert)

<!-- -->

-   `exec(stmt)`:
    -   führt Statements aus (Zustand, Nebeneffekte)
    -   zuletzt ausgewertetes `ExprStmt` ist Rückgabewert des Interpreters

<!-- -->

-   Block-Ausführung:
    -   beim `BlockStmt` neues `Env(env)` anlegen
    -   nach dem Block `env` auf `parent` zurücksetzen
-   `VarDecl`: Initialisierung evaluieren, Name und Wert im aktuellen `Env` mit `define(name, value)` eintragen
-   `VarExpr`:
    -   über vom Name-Resolver berechnete Tiefe das passende `Env` finden, und
    -   Wert mit `get(name, depth)` lesen

> [!TIP]
>
> <details open>
> <summary><strong>📖 Zum Nachlesen</strong></summary>
>
> Zur Interpretation von Statements kann man in ([Nystrom 2021](#ref-Nystrom2021)) in Kapitel 8 "Statements and State" nachlesen, dort noch ohne Tiefeninformation. In Kapitel 11 "Resolving and Binding" wird dann die Namensauflösung und Berechnung der Tiefeninformation eingeführt. Dabei behandelt Nystrom ([2021](#ref-Nystrom2021)) die Verbindung zwischen Name-Resolver und Interpreter technisch anders als wir, aber das Grundprinzip der Namensauflösung ist ähnlich wie hier gezeigt.
>
> </details>

> [!NOTE]
>
> <details >
> <summary><strong>✅ Lernziele</strong></summary>
>
> -   k2: Ich kann den Unterschied zwischen Symboltabellen (`Scope`) und Environments (`Env`) erklären
> -   k2: Ich kann beschreiben, wie lexikalische Scopes zur Laufzeit durch verschachtelte Environments modelliert werden
> -   k3: Ich kann eine `Env`-Struktur in Java implementieren, die Namen und Werte mit Eltern-Environments verwaltet
> -   k3: Ich kann einen bestehenden Ausdrucksinterpreter um die Ausführung von Statements mit `exec(Stmt)` erweitern
> -   k3: Ich kann Variablenzugriffe (`VarExpr`) so implementieren, dass die vom Name-Resolver berechnete Tiefe zur Auswahl des richtigen Environments verwendet wird
> -   k4: Ich kann anhand eines gegebenen Programms den Zustand des Environments zu ausgewählten Programmpunkten analysieren und begründen
>
> </details>

> [!IMPORTANT]
>
> <details open>
> <summary><strong>🏅 Challenges</strong></summary>
>
> **Nested Scopes**
>
> Betrachten Sie folgendes Lispy-Programm in der Sprachstufe L-Var:
>
> ``` clojure
> (def x : int 10)
> (def y : int 100)
>
> (let ((x : int 1))
>   (+ x y)
>   (let ((y : int 2))
>     (+ x y))
>   (+ x y))
> ```
>
> -   Zeichnen Sie zu den folgenden Programmpunkten jeweils den Zustand der Environments und geben Sie die Werte von `x` und `y` an:
>     -   direkt nach `def x`
>     -   nach Eintritt in den ersten `let`‑Block (noch vor dem ersten `(+ x y)`)
>     -   nach Eintritt in den inneren `let`‑Block (vor `(+ x y)` im inneren Block)
>     -   nach Verlassen des inneren `let`‑Blocks (vor dem letzten `(+ x y)` im äußeren Block)
> -   Erläutern Sie kurz, wie der `BlockStmt`‑Fall in `exec(stmt)` dafür sorgt, dass die richtigen Environments angelegt und wieder verlassen werden.
>
> **Mini-C++: Implementieren von `assign()` im Environment**
>
> Wenn es wie in Mini-C++ Zuweisungen in der Sprache gibt, dann braucht man eine Möglichkeit, um Namen in einem `Env` einen neuen Wert zuzuweisen.
>
> Für diese Neuzuweisung von Namen in Mini-C++ kann man sich eine Methode `void assign(String name, Value value, int depth)` definieren, die analog zu `get()` das Elternenvironment in der Tiefe `depth` aufsucht und dort dem Namen `name` den neuen Wert `value` zuweist. Skizzieren Sie diese Methode.
>
> Beachten Sie die Semantik: Bei einer Zuweisung wie `c = 42;` muss die Variable vom aktuellen Scope aus sichtbar sein, aber sie muss nicht im aktuellen Scope selbst definiert sein. Im Name-Resolver hatten wir bereits die Sichtbarkeit geprüft und berechnet, wie viele Schritte man in der Scope-/Environment-Kette nach "oben" gehen muss bis zum Scope/Environment, in dem dieser Name definiert wurde. Nutzen Sie diese Information.
>
> **Mini-C++: Implementieren von `AssignStmt` im Interpreter**
>
> In L-Var gibt es keine Zuweisung, deshalb wurde die Behandlung von `AssignStmt` in der Methode `exec(Stmt stmt)` ausgelassen. In Mini-C++ existiert die Zuweisung - skizzieren Sie diesen Fall in Java.
>
> Welche Elemente hat ein `AssignStmt`-Knoten (wie sah der entsprechende Teil der konkreten und abstrakten Syntax aus)? Gab es Einschränkungen für die linke Seite der Zuweisung? Wie sollte die Zuweisung durchgeführt werden?
>
> **Fehlerfälle - Was passiert, wenn der Name-Resolver "versagt"?**
>
> Angenommen, in Ihrem L-Var-Compiler gäbe es (hypothetisch) einen Bug im Name-Resolver, sodass für einige `VarExpr`-Knoten keine oder eine falsche Tiefe in `sema.depths` eingetragen wird.
>
> 1.  Diskutieren Sie anhand eines kleinen Beispiels:
>
>     ``` clojure
>     (def x : int 1)
>     (let ((y : int 2))
>       (+ x y))
>     ```
>
>     was passieren könnte, wenn:
>
>     -   `depth` für beide `VarExpr` fälschlicherweise 0 wäre,
>     -   `depth` für `x` korrekt und für `y` zu groß ist.
>
> 2.  Überlegen Sie, welche zusätzlichen Prüfungen man in `Env#get` und `Env#define` ergänzen könnte, um solche internen Inkonsistenzen zu erkennen.
>
> 3.  Formulieren Sie, wie eine Fehlermeldung in diesem Fall aussehen sollte, damit klar ist, dass es sich um einen **internen Fehler** des Compilers (und nicht um einen Fehler im Programm der User) handelt.
>
> **`Scope` vs. `Env`**
>
> Im Skript wurde behauptet, dass man zum Speichern der Werte während der Interpretation neue Datenstrukturen benötigt und nicht die bereits in der semantischen Analyse aufgebauten Symboltabellen dafür nutzen kann.
>
> Demonstrieren Sie an einem praktischen Beispiel, warum dies so ist.
>
> **Nested Environments vs. Stack von Environments**
>
> Unsere Environments sind analog zu den Scopes aufgebaut und haben einen Verweis auf das Eltern-Environment. Damit kann man einen Namen rekursiv vom aktuellen Environment aus nach "oben" bis zur Wurzel (globales Environment) suchen.
>
> In vielen Fällen könnte man auf diese Verzeigerung verzichten und im Interpreter einfach einen Stack von einfachen Environments pflegen - jeder neue Scope legt ein neues Environment oben auf den Stack, jedes Verlassen eines Scopes entfernt den obersten Eintrag. Das funktioniert sogar für Funktionsaufrufe!
>
> Es gibt aber Szenarien, wo dies nicht ausreicht. Wir werden möglicherweise für unser Lispy so ein Szenario sehen. Können Sie sich selbst ein Szenario überlegen, wo ein `Stack<Env>` im Interpreter nicht reicht und wir wissen müssen, was das Eltern-Environment `Env parent` ist?
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
> </div>
>
> </details>

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> d887ffd 2026-11-24 lvar: finalize lesson (environments)<br></sub></sup></p></blockquote>
