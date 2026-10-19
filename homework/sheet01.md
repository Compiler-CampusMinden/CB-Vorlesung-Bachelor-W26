# Blatt 01: Reguläre Sprachen

## A1.1: Sprachen von regulären Ausdrücken

Welche Sprache wird von dem folgenden regulären Ausdruck beschrieben?

$a\ +\ a\ (a\ +\ b)^*\ a$

## A1.2: Bezeichner in Programmiersprachen

Die Referenz einer Programmiersprache beschreibt zugelassene Bezeichner (= Namen für Variablen, Funktionen, Klassen, Methoden, ...) folgendendermaßen:

-   Alle Variablennamen beginnen mit **V** oder **v**
-   Handelt es sich um globale Variablen, beginnen Sie mit **V**, lokale beginnen mit **v**
-   Funktions- und Methodenparameter beginnen mit **p**, KLassenparameter (bei der Definition von Vererbung) beginnen mit **P**
-   Weitere Bezeichner müssen mit einem Buchstaben (a-z, A-Z) beginnen
-   Die folgenden Zeichen dürfen Buchstaben, Ziffern und ein Unterstrich sein
-   Bezeichner dürfen nicht mit einem Unterstrich enden
-   Alle Bezeichner müssen aus mindestens zwei Zeichen bestehen

Entwickeln Sie **einen** regulären Ausdruck, der den Aufbau aller Bezeichner beschreibt.

Beachten Sie, dass Ihr regex alle zulässigen Bezeichner beschreiben muss, aber keinen einzigen unzulässigen beschreiben darf. Wählen Sie zwei Bezeichner aus der Sprache und zeigen Sie, wie sie vom regex gematcht werden.

Entwickeln Sie einen DFA, der diese Bezeichner akzeptiert. Beachten Sie, dass Ihr DFA alle zulässigen Bezeichner akzeptieren muss, aber keinen einzigen unzulässigen akzeptieren darf. Wählen Sie zwei Bezeichner aus der Sprache und zeigen Sie, wie sie vom Automaten zeichenweise gelesen und akzeptiert werden.

Bitte beachten Sie, dass die Schreibweise a-z nicht unserer Definition von DFAs genügt. Eigentlich müsste jedes Zeichen aufgeführt werden.

Anmerkung: Diese Darstellung wird ab jetzt akzeptiert.

Entwickeln Sie eine reguläre Grammatik, die diese Bezeichner generiert. Beachten Sie, dass Ihre Grammatik alle zulässigen Bezeichner generieren können muss, aber keinen einzigen unzulässigen generieren darf. Wählen Sie zwei Bezeichner aus der Sprache und zeigen Sie die Ableitungsbäume dazu.

## A1.3: Gleitkommazahlen in Programmiersprachen

Recherchieren Sie zunächst den Aufbau von Gleitkommazahlen in Python und Java.

Erstellen Sie für jede der beiden Programmiersprachen reguläre Ausdrücke, DFAs und reguläre Grammatiken wie in Aufgabe A1.2. Verifizieren Sie Ihre Lösungen wie in Aufgabe A1.2. Vorgaben, die sich auf Längen oder Werte von Teilen der Zahlen beziehen, ignorieren Sie bitte.

## A1.4: Mailadressen?

Warum ist der folgende regex ungeeignet für die Verarbeitung von Mailadressen?

$(a-z)^+@(a-z).(a-z)$

Verbessern Sie den gegebenen regulären Ausdruck.

## A1.5: Der zweitletzte Buchstabe

Entwickeln Sie einen DFA, der nur Wörter über $\Sigma = \lbrace 1,2 \rbrace$ akzeptiert, deren zweites und zweitletztes Zeichen eine **1** ist.

## A1.6: Sprache einer regulären Grammatik

Welche Sprache generiert die folgende Grammatik?

$$\begin{eqnarray}
S &\rightarrow& a A \ | \ b B            \nonumber \\
A &\rightarrow& b S \ | \ c              \nonumber \\
B &\rightarrow& a S \ | \ b              \nonumber
\end{eqnarray}$$

Können Sie einen regulären Ausdruck oder einen DFA dafür angeben?

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> 897f91d 2026-10-19 sheet01: reformat markdown<br></sub></sup></p></blockquote>
