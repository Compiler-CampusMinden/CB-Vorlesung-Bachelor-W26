# Blatt 02: CFG

## A2.1: PDA (3P)

Erstellen Sie einen deterministischen PDA, der die Sprache

$$L = \lbrace w \in \lbrace a, b, c \rbrace^* \; | \; w \; \text{hat doppelt so viele a's wie c's} \rbrace$$

akzeptiert.

Beschreiben Sie Schritt für Schritt, wie der PDA die Eingaben *bcaba*
und *bccac* abarbeitet.

## A2.2: Akzeptierte Sprache (2P)

Ist der folgenden PDA deterministisch? Warum (nicht)?

$q_4$ sei der akzeptierende Zustand.

$$\begin{eqnarray}
\delta(q_0,a, \perp) &=& (q_0, A\perp)           \nonumber \\
\delta(q_0,a, A) &=& (q_0, AA)                   \nonumber \\
\delta(q_0,b, A) &=& (q_1, BA)                   \nonumber \\
\delta(q_1,b, B) &=& (q_1, BB)                   \nonumber \\
\delta(q_1,c, B) &=& (q_2, \epsilon)             \nonumber \\
\delta(q_2,c, B) &=& (q_2, \epsilon)             \nonumber \\
\delta(q_2,d, A) &=& (q_3, \epsilon)             \nonumber \\
\delta(q_3,d, A) &=& (q_3, \epsilon)             \nonumber \\
\delta(q_3,d, A) &=& (q_3, AA)                   \nonumber \\
\delta(q_3,\epsilon, \perp) &=& (q_4, \epsilon)  \nonumber
\end{eqnarray}$$

Zeichnen Sie den Automaten. Geben Sie das 7-Tupel des PDa an. Welche
Sprache akzeptiert er?

## A2.3: Kontextfreie Sprache (2P)

Welche Sprache generiert die folgende kontextfreie (Teil-) Grammatik?

$$G = (\lbrace \text{Statement}, \text{Condition}, \ldots \rbrace, \lbrace \text{"if"}, \text{"else"}, \ldots \rbrace, P, \text{Statement})$$

mit

$$\begin{eqnarray}
P = \lbrace &&                                                                                                           \nonumber \\
&\text{Statement}& \rightarrow \text{"if" Condition Statement} \; | \; \text{"if" Condition Statement "else" Statement}  \nonumber \\
&\text{Condition}& \rightarrow \ldots                                                                                    \nonumber \\
\rbrace                                                                                                                  \nonumber
\end{eqnarray}$$

Ist die Grammatik mehrdeutig? Warum (nicht)?

## A2.4: Kontextfreie Grammatik (3P)

Entwickeln Sie eine kontextfreie Grammatik für die Sprache

$$L = \lbrace a^ib^jc^k \; | \; i = j \lor j = k \rbrace$$

Zeigen Sie, dass die Grammatik mehrdeutig ist. Entwickeln Sie einen PDA
für diese Sprache.

------------------------------------------------------------------------

<p align="center"><img src="https://licensebuttons.net/l/by-sa/4.0/88x31.png"  /></p>

Unless otherwise noted, this work is licensed under CC BY-SA 4.0.

<blockquote><p><sup><sub><strong>Last modified:</strong> 1fcd90d 2026-09-03 update homework<br></sub></sup></p></blockquote>
