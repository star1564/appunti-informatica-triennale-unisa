---
aliases:
tags:
  - corsi/matematica/analisi
paragrafo: Matematica di Base
cssclasses:
  - 
---
>Un **Sistema di Equazioni Lineari** è un gruppo di [[MB - 04 Equazioni di Primo Grado|equazioni di primo grado]] con ciascuna avente *una o più incognite*. Hanno la seguente forma:
>$$\begin{cases} \text{equazione}_1 \\ ...\\ \text{equazione}_n \end{cases}$$
>
>I sistemi lineari hanno solitamente un *numero di equazioni uguale al numero di incognite*. Queste sono della forma:
>$$\text{Due equazioni e due incognite}=\begin{cases} a_1\textcolor{#FF6611}x + b_1\textcolor{#FF6611}y = c_1 \\ a_2\textcolor{#FF6611}x + b_2\textcolor{#FF6611}y = c_2 \end{cases}$$
>$$\text{Tre equazioni e tre incognite}=\begin{cases} a_1\textcolor{#FF6611}x + b_1\textcolor{#FF6611}y + c_1\textcolor{#FF6611}z = d_1 \\ a_2\textcolor{#FF6611}x + b_2\textcolor{#FF6611}y + c_2\textcolor{#FF6611}z = d_2 \\ a_3\textcolor{#FF6611}x + b_3\textcolor{#FF6611}y + c_3\textcolor{#FF6611}z = d_3 \end{cases}$$
>Dove:
>- $\color{#FF6611}x$, $\color{#FF6611}y$ e $\color{#FF6611}z$ sono le [[MB - 00 Equazioni e Uguaglianze#Variabili (Incognite)|incognite]]
>- Gli altri numeri sono delle [[MB - 00 Equazioni e Uguaglianze#Costanti|costanti]] coefficienti

> [!example]+ <font color="orange">Esempio</font>
>- $\begin{cases} x+y=0 \\ x-y=3 \end{cases}$
>- $\begin{cases} 2x+3y-z=-2 \\ x-y=2 \end{cases}$
>- $\dots$

Lo scopo di un sistema di equazioni consiste nell'individuare tutte e sole le possibili soluzioni che **risolvono contemporaneamente** tutte le equazioni del sistema. Quindi:
- in un sistema con due equazioni e due incognite, la soluzione del sistema è una coppia di valori reali che soddisfano allo stesso tempo entrambe le equazioni del sistema $$(r_1, r_2),\quad r_1,r_2 \in \mathbb{R}$$
- in un sistema con tre equazioni e tre incognite, la soluzione del sistema è una tripla di valori reali che soddisfano allo stesso tempo tutte le equazioni del sistema $$(r_1, r_2, r_3),\quad r_1,r_2,r_3 \in \mathbb{R}$$

Graficamente, la soluzione rappresenta il **punto** dove *tutte le rette* (rappresentate dalle equazioni) del sistema *si incontrano*.

> [!example]- <font color="orange">Esempio</font>
>$$\begin{cases} x+y=24 \\ x-y=6 \end{cases}$$
>
>![[Pasted image 20240924161518.png|500]]
>
>La soluzione del sistema è il punto con coordinate $(15,9)$.


## Possibili soluzioni
1. Se *almeno un'equazione del sistema è [[MB - 04 Equazioni di Primo Grado#^55783a|impossibile]]*, allora il sistema non ammette alcuna soluzione e si dice **Impossibile**.
2. Se *esistono infinite soluzioni*, allora il sistema si dice **Indeterminato**.
3. Se *esiste una e una sola soluzione* (ossia una e una sola $n$-upla di valori che, sostituiti ordinatamente alle incognite, risolvono tutte le equazioni), allora il sistema si dice **Determinato**.

## Metodo di Sostituzione per Sistemi di Equazioni Lineari
Il **metodo di sostituzione** è un procedimento algebrico utilizzato per risolvere sistemi di equazioni e di [[MB - 02 Disequazioni e Disuguaglianze|disequazioni]]:

1. Scegliere una delle equazioni del sistema e isolare una variabile (come $x$ o $y$). Di solito si sceglie l'equazione più semplice da risolvere.
2. Prendere l'espressione ottenuta per la variabile isolata e sostituirla nell'altra equazione.
3. Risolvere la nuova equazione ottenuta per l'unica variabile rimasta.
4. Sostituire il valore trovato per risolvere la variabile isolata nell'equazione originale.
5. Il sistema è risolto.

Se il sistema è con tre espressioni e due incognite, bisogna procedere come sopra ma iterare per ottenere un sistema con due equazioni e due incognite all'interno del sistema originale.

> [!example]- <font color="orange">Esempio con due espressioni e due incognite</font>
>$$\begin{cases} x+y=5 \\ x-y=1 \end{cases}$$
>
>1. $\begin{cases} \color{#CC241D}x=5-y \\ x-y=1 \end{cases}$
>2. $\begin{cases} x=5-y \\ \textcolor{#CC241D}{(5-y)}-y=1 \end{cases}$
>3. $\begin{cases} x=5-y \\ \color{#CC241D}-2y=-4 \end{cases}\Rightarrow\begin{cases} x=5-y \\ \color{#CC241D}y=2 \end{cases}$
>4. $\begin{cases} x=5-\color{#CC241D}(2) \\ y=2 \end{cases}$
>5. $\begin{cases} x=3 \\ y=2 \end{cases} \Longrightarrow (3,2) \text{ soluzione}$
>
>Verifica:
>$$\begin{cases} 3+2=5 \\ 3-2=1 \end{cases} \Longrightarrow \text{vero}$$

> [!example]- <font color="orange">Esempio con tre espressioni e tre incognite</font>
>$$\begin{cases} x+y-z=0 \\ x-y+z=1 \\ x+xy+3z=6 \end{cases}$$
>
>1. $\begin{cases} \color{#CC241D}x=z-y \\ x-y+z=1 \\ x+xy+3z=6 \end{cases}$
>2. $\begin{cases} x=z-y \\ \textcolor{#CC241D}{(z-y)}-y+z=1 \\ \textcolor{#CC241D}{(z-y)}+2y+3z=6 \end{cases}$
>3. $\begin{cases} x=z-y \\ \color{#CC241D}2z-2y=1 \\ \color{#CC241D}4z+y=6 \end{cases}$
>4. Risolvere il sistema a due equazioni e due incognite trovato: $\begin{cases} \dots \\ 2z-2y=1 \\ 4z+y=6 \end{cases}$
>5. $\begin{cases} \dots \\ 2z-2y=1 \\ \color{#CC241D}y=6-4z \end{cases}$
>6. $\begin{cases} \dots \\ \color{#CC241D}2z-2(6-4z)=1 \\ y=6-4z \end{cases} \Longrightarrow \begin{cases} \dots \\ \color{#CC241D}2z-12 + 8z=1 \\ y=6-4z \end{cases} \Longrightarrow \begin{cases} \dots \\ \color{#CC241D}10z=13 \\ y=6-4z \end{cases}\Longrightarrow \begin{cases} \dots \\ \color{#CC241D}z= \frac{13}{10} \\ y=6-4z \end{cases}$
>7. $\begin{cases} \dots \\ z= \frac{13}{10} \\ \color{#CC241D}y=6-4\left( \frac{13}{10} \right) \end{cases} \Longrightarrow \begin{cases} \dots \\ z= \frac{13}{10} \\ \color{#CC241D}y=6-\frac{26}{5} \end{cases}\Longrightarrow \begin{cases} \dots \\ z= \frac{13}{10} \\ \color{#CC241D}y=\frac{30-26}{5}=\frac{4}{5} \end{cases}$
>8. $\begin{cases} \color{#CC241D}x= \left( \frac{13}{10} \right)- \left( \frac{4}{5} \right)= \frac{13-8}{10}=\frac{5}{10}=\frac{1}{2} \\ z= \frac{13}{10} \\ y=\frac{4}{5} \end{cases}$
>9. $\begin{cases} x= \frac{1}{2} \\ z= \frac{13}{10} \\ y=\frac{4}{5} \end{cases} \Longrightarrow \left( \frac{1}{2}, \frac{4}{5}, \frac{13}{10} \right) \text{ soluzione}$

---
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/equazioni/164-sistemi-di-equazioni-lineari-di-primo-grado.html)