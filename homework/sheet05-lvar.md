# Blatt 05: L-Var und C-Var (Variablen, State, Scopes)

## A5.1: L-Var: Scopes und Tiefenwerte verfolgen (\*)

Betrachten Sie das folgende L-Var-Programm:

``` clojure
(def x : int 10)

(let ((y : int 2))
  (+ x y)
  (let ((x : int 5))
    (+ x y))
  (+ x y))
```

Skizzieren Sie die Arbeit des Name-Resolvers:

1.  Zeichnen Sie den Scope-Baum und tragen Sie in jeden Scope die dort definierten Variablen ein.
2.  Gehen Sie die Statements und Expressions in der Reihenfolge durch, in der der Resolver sie besucht. Notieren Sie dabei, wann ein neuer Scope betreten und wieder verlassen wird.
3.  Ordnen Sie jede `VarExpr` der passenden `VarDecl` zu.
4.  Geben Sie für jede `VarExpr` den von `scope.resolve(name)` gelieferten `depth`-Wert an.

## A5.2: L-Var: `NameResolverLvar#resolve()` ergänzen (\*)

Sie haben im Skript für L-Var den Name-Resolver `NameResolverLvar` kennengelernt. Dabei wurde die Implementierung für die AST-Knoten `ExprStmt` (`Stmt`) sowie `SubExpr`, `MulExpr` und `DivExpr` (alle drei `Expr`) nicht gezeigt.

<details>

``` java
public class NameResolverLvar {
    protected SemaInfo sema;
    protected Scope scope;

    public SemaInfo resolve(List<Stmt> stmts, SemaInfo semaInfo) {
        sema = semaInfo;  scope = sema.globalScope;
        stmts.forEach(this::resolve);
        return sema;
    }

    protected void resolve(Stmt stmt) {
        sema.symbols.put(stmt, scope);

        switch (stmt) {
            case VarDecl(var name, var type, var init) -> {
                init.ifPresent(this::resolve);
                resolve(type);
                scope.bind(name, stmt);
            }
            case BlockStmt(var s) -> {
                scope = new Scope(scope);
                s.forEach(this::resolve);
                scope = scope.parent();
            }
            default -> throw new RuntimeException("unexpected statement");
        }
    }

    protected void resolve(Expr expr) {
        switch (expr) {
            case IntLiteral _ -> { } // nothing to do
            case AddExpr(var l, var r) -> List.of(l, r).forEach(this::resolve);
            case VarExpr e -> {
                var res = scope.resolve(e.name());
                sema.decls.put(e, res);
            }
            default -> throw new RuntimeException("unexpected expression");
        }
    }
}
```

</details>

Skizzieren Sie die fehlenden Fälle in `resolve(stmt)` und `resolve(expr)` für `ExprStmt`, `SubExpr`, `MulExpr` und `DivExpr` in Java.

## A5.3: L-Var: Von `depth` zum Laufzeitwert (\*)

Betrachten Sie erneut das Programm von oben:

``` clojure
(def x : int 10)

(let ((y : int 2))
  (+ x y)
  (let ((x : int 5))
    (+ x y))
  (+ x y))
```

Der Name-Resolver hat für jede `VarExpr` eine Tiefe berechnet. Der Interpreter verwendet diese Tiefe, um vom aktuellen `Env` aus das passende Environment auszuwählen.

1.  Skizzieren Sie die Environments während der Ausführung schrittweise.
2.  Erklären Sie anhand des inneren `(+ x y)`, warum `x` und `y` über unterschiedliche Tiefen aufgelöst werden.
3.  Was würde im Interpreter schiefgehen, wenn für das innere Vorkommen von `x` fälschlich die Tiefe des globalen `x` gespeichert wäre?

## A5.4: C-Var: Variablen, State und nested Scopes (\*\*)

Wir erweitern **C-Expr aus dem letzten Übungsblatt zu C-Var**. Dabei ergänzen wir u.a. Variablendeklarationen, Zuweisungen und Blöcke mit eigenen Scopes.

> [!TIP]
>
> Nutzen Sie Ihre bestehende **ANTLR-Pipeline** aus C-Expr als Ausgangspunkt. Erweitern Sie die ANTLR-Grammatik, leiten Sie den AST-Builder und die übrigen Komponenten von den entsprechenden C-Expr-Bausteinen ab und ergänzen Sie die neuen Fälle. Ein handgeschriebener Lexer oder RD-Parser ist ab diesem Blatt nicht (mehr) erforderlich.

> [!IMPORTANT]
>
> ### Waschzettel: In (Mini-) C++ sieht es so aus ...
>
> | Zweck | L-Var (Lispy) | C-Var (Mini-C++) |
> |:---------------------|:-----------------------------|:--------------------|
> | Variable definieren (mit Initialisierung) | `(def x : int 10)` | `int x = 10;` |
> | Variable definieren (Defaultwert) | `(def x : int)` | `int x;` |
> | Zuweisung an Variablen | n.a. | `x = 2;` |
> | Variable lesen (Teil eines Ausdruck) | `x` | `x` |
> | Nested Scopes | `(def x : int 10) (let ((a : int 2) (x : int 3)) (+ a x))` | `int x=10; { int a=2; int x=3; a+x; }` |
>
> Ein `let` öffnet in Lispy einen neuen lokalen Scope mit lokalen Variablen:
>
> ``` clojure
> (def x : int 10)
> (let ((a : int 2)
>       (x : int 3))
>   (+ a x))
> ```
>
> In C++ nutzen wir dafür die geschweiften Klammern:
>
> ``` cpp
> int x=10;
> {
>     int a=2;
>     int x=3;
>     a+x;
> }
> ```
>
> Für C-Var gelten die folgenden Festlegungen:
>
> -   Variablen haben den Typ `int` und können bei der Definition mit einem Ausdruck initialisiert werden.
> -   Eine Variable darf in ihrer Initialisierung nicht auf sich selbst verweisen.
> -   Eine Variable darf innerhalb desselben Scopes nur einmal definiert werden.
> -   Eine Definition in einem inneren Scope darf eine Definition in einem äußeren Scope verdecken (*shadowing*).
> -   Variablenzugriffe werden lexikalisch aufgelöst.
> -   Es darf keine Zugriffe (lesend, schreibend) auf nicht sichtbare Variablen geben.
> -   Zuweisungen dürfen in C-Var nur an Variablen erfolgen.
> -   Der Name-Resolver speichert für jede Variablenreferenz (auch für die Zielvariable einer Zuweisung!) die Tiefe, die der Interpreter später zum Zugriff auf das passende Environment verwendet.
> -   Ein C-Var-Programm besteht aus einer **Folge von Statements**.
> -   **Abweichungen** vom "echten" C++:
>     -   Zuweisungen ergeben in Mini-C++ keinen Wert (sind also Statements und nicht Expressions).
>     -   Der Wert des zuletzt ausgewerteten Expression-Statements ist der Rückgabewert des Interpreters.
>     -   Der Interpreter nutzt für Integer-Variablen den Defaultwert `0`, falls eine Variable nicht vom User initialisiert wird.

### Schritt 1: C-Var ausprobieren

Wir wollen Variablendeklarationen, Zuweisungen und Blockverhalten zunächst direkt in C++ ausprobieren.

Schreiben Sie ein C++-Programm `var.cpp` mit dem üblichen `main()`-Block und den folgenden Statements:

``` cpp
int x = 10;
x = x + 1;
x + 2;
{
    int y = 3;
    x + y;
}
```

Ergänzen Sie an geeigneten Stellen `std::cout`-Ausgaben, sodass die Werte vor und nach der Zuweisung sowie im und nach dem Block sichtbar werden.

Übersetzen Sie es mit g++/clang und prüfen Sie, ob jeweils das Ergebnis Ihren Erwartungen entspricht.

### Schritt 2: C-Var-Grammatiken und AST ergänzen

1.  Ergänzen Sie die **konkrete Grammatik** um passende Regeln für die neuen Sprachkonstrukte. Orientieren Sie dabei am Vorgehen im Skript.

2.  Leiten Sie aus der konkreten Grammatik die notwendigen Ergänzungen Ihrer **ANTLR-Grammatik** aus C-Expr ab.

3.  Ergänzen Sie die **abstrakte Grammatik** und leiten Sie daraus die neuen Knotentypen für den AST ab.

4.  Leiten Sie Ihren C-Expr-AST-Builder zu einem **C-Var-AST-Builder** ab. Überschreiben Sie die benötigten `visitXYZ()`-Methoden und erläutern Sie kurz, welche bisherigen Methoden Sie weiterverwenden können.

### Schritt 3: Name-Resolver und Scopes

Implementieren Sie einen `NameResolverCvar`. Orientieren Sie sich am Vorgehen im Skript.

Testen Sie den Resolver mindestens mit diesen Fällen:

``` cpp
int x = 1;
x + 2;
```

``` cpp
int x = 1;
{
    int y = x + 2;
    y + x;
}
```

``` cpp
int x = 1;
{
    int x = 2;
    x + 1;
}
```

Prüfen Sie für diese Tests, ob die richtigen Namen im richtigen Scope gebunden wurden und die korrekte Tiefeninformation berechnet wurde.

Erstellen Sie außerdem noch **Negativtests**: Welche Eingaben sollte der Name-Resolver zurückweisen?

### Schritt 4: Interpreter und Environments

Erweitern Sie Ihren C-Expr-Interpreter zu einem `InterpreterCvar`. Ergänzen Sie eine `Env`-Struktur, die Namen und Laufzeitwerte speichert und auf das Eltern-Environment verweist.

Erläutern Sie, welche Informationen der Interpreter aus der Sidetable benötigt.

Legen Sie fest, welchen Wert Ihr Interpreter für ein Programm zurückgibt. Orientieren Sie sich dabei an der L-Var-Semantik: Der Wert des letzten ausgewerteten Expression-Statements wird zurückgegeben; falls keines vorkommt, wird ein Einheitswert zurückgegeben.

Ergänzen Sie Ihre JUnit-Tests so, dass die neuen Sprachkonstrukte in der Pipeline getestet werden.

Vergleichen Sie das beobachtbare Verhalten Ihres Interpreters mit einem entsprechenden C++-Programm (clang/g++). Welche Unterschiede ergeben sich durch die Festlegungen von Mini-C++?

## A5.5: Vertiefung: Namensauflösung (\*\*\*)

Betrachten Sie:

``` cpp
int x = 1;
{
    int x = x + 1;
}
```

Was passiert bei der Namensauflösung des `x` im Initialisierungsausdruck? Vergleichen Sie das mit dem Verhalten des "echten" C++.

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> a73f193 2026-11-28 lvar: improve sidetable (sheet 05)<br></sub></sup></p></blockquote>
