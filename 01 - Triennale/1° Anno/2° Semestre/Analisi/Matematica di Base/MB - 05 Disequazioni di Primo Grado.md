---
tags:
  - corsi/matematica/analisi
paragrafo: Matematica di Base
cssclasses:
  - 
---
La definizione è analoga a quella dell'[[MB - 04 Equazioni di Primo Grado|equazione di primo grado]].

>Una **Disequazione di primo grado** in *una incognita* è una [[MB - 02 Disequazioni e Disuguaglianze|disequazione]] *riducibile* ad una delle seguenti forme:
>$$\begin{align} \\\textcolor{#00C575}{a}\textcolor{#FF6611}{x}+\textcolor{#00C575}{b}>0 \\\textcolor{#00C575}{a}\textcolor{#FF6611}{x}+\textcolor{#00C575}{b}\geq 0 \\ \textcolor{#00C575}{a}\textcolor{#FF6611}{x}+\textcolor{#00C575}{b}<0 \\ \textcolor{#00C575}{a}\textcolor{#FF6611}{x}+\textcolor{#00C575}{b}\leq0\\ \end{align}$$
>
>Che equivalgono a: $$\textcolor{#00C575}{a}\textcolor{#FF6611}{x}+\textcolor{#00C575}{b}\lesseqgtr0$$
>
>Queste vengono dette *forme normali delle disequazioni di primo grado*, in cui: 
>- $\textcolor{#00C575}{a}$ e $\textcolor{#00C575}{b}$ sono delle [[MB - 00 Equazioni e Uguaglianze#Costanti|costanti]] coefficienti
>- $\textcolor{#FF6611}{x}$ è l'[[MB - 00 Equazioni e Uguaglianze#Variabili (Incognite)|incognita]], oggetto della risoluzione della disequazione di primo grado, di cui bisogna individuare i valori che rendono vera la disuguaglianza
>
>In questo tipo di disequazione, l'incognita $\textcolor{#FF6611}{x}$ è elevata a *esponente 1* (grado 1), può essere solamente moltiplicata per un coefficiente $\textcolor{#00C575}{a}$ qualsiasi e può essere sommata ad un qualsiasi coefficiente $\textcolor{#00C575}{b}$.

> [!example]+ <font color="orange">Esempio</font>
>- $x+1\geq0$
>- $-x+6\le2$
>- $x+1<2x-1$

## Possibili soluzioni
Si utilizzano gli stessi metodi per [[MB - 04 Equazioni di Primo Grado#Risolvere Equazioni di Primo Grado|risolvere un'equazione di primo grado]], tenendo in considerazione i [[MB - 03 Principi di Equivalenza delle Disequazioni|Principi di Equivalenza delle Disequazioni]].

### 1) L'incognita scompare
Se, svolgendo i calcoli, *l'incognita scompare*, si avrà che la disequazione si riduce ad una disuguaglianza tra due numeri, del tipo: $$0\lesseqgtr \text{numero}$$
A seconda del verso del simbolo della disequazione e del valore del numero, la disuguaglianza può essere vera o falsa:

- Se la disuguaglianza numerica è *falsa*, la disequazione è **Impossibile** e non ammette alcuna soluzione.
   In questo caso si scrive che $\color{#61AFEF}\not\exists x\in \mathbb{R}$. ![[Pasted image 20240924122256.png|450]]

- Se la disuguaglianza numerica è *vera*, la disequazione è **Indeterminata** e ammette un numero infinito di soluzioni.
   In questo caso si scrive che $\color{#61AFEF}\forall x\in \mathbb{R}$. ![[Pasted image 20240924122510.png|450]]

### 2) L'incognita rimane
Se, svolgendo i calcoli, *l'incognita non scompare*, si avrà che la disequazione si riduce ad una delle forme normali delle disequazioni di primo grado, con $$a\neq b \land a\neq 0 \iff x\lesseqgtr \frac{b}{a}$$
Dopo aver ottenuto una delle forme normali, si può ricavare l'insieme delle soluzioni attraverso il seguente schema.

| Simbolo della Disequazione $ax\lesseqgtr b$ |      Segno di $a$      |        Insieme delle soluzioni        |
| :-----------------------------------------: | :--------------------: | :-----------------------------------: |
|          $\textcolor{#00C575}\gt$           | $\textcolor{#61AFEF}+$ | $x\textcolor{#00C575}\gt \frac{b}{a}$ |
|          $\textcolor{#00C575}\gt$           | $\textcolor{#CC241D}-$ | $x\textcolor{#FF6611}\lt \frac{b}{a}$ |
|          $\textcolor{#00C575}\ge$           | $\textcolor{#61AFEF}+$ | $x\textcolor{#00C575}\ge \frac{b}{a}$ |
|          $\textcolor{#00C575}\ge$           | $\textcolor{#CC241D}-$ | $x\textcolor{#FF6611}\le \frac{b}{a}$ |
|          $\textcolor{#FF6611}\lt$           | $\textcolor{#61AFEF}+$ | $x\textcolor{#FF6611}\lt \frac{b}{a}$ |
|          $\textcolor{#FF6611}\lt$           | $\textcolor{#CC241D}-$ | $x\textcolor{#00C575}\gt \frac{b}{a}$ |
|          $\textcolor{#FF6611}\le$           | $\textcolor{#61AFEF}+$ | $x\textcolor{#FF6611}\le \frac{b}{a}$ |
|          $\textcolor{#FF6611}\le$           | $\textcolor{#CC241D}-$ | $x\textcolor{#00C575}\ge \frac{b}{a}$ |


> [!example]+ <font color="orange">Esempio di risoluzione e di prova</font>
>$$x+1<2x-1$$
>
>$$
>\begin{aligned}
>x+1 &< 2x-1 \\
>x+1 \textcolor{#CC241D}{-1} &< 2x-1\textcolor{#CC241D}{-1} \\
>x &< 2x-2 \\
>x\textcolor{#CC241D}{-2x} &< 2x\textcolor{#CC241D}{-2x}-2 \\
>-x &< -2 \\
>\textcolor{#CC241D}{-1}(-x) &\textcolor{#fdaa39}< \textcolor{#CC241D}{-1}(-2) \\
>x &> 2 \\
>\end{aligned}
>$$
>
>
>Sostituendo $x$ ad un numero $>2$ (ad esempio $3$) nella disequazione originale otteniamo
>$$
>\begin{align}
>3+1&<2(3)-1 \\
>4&<6-1 \\
>4&<5 \\
>&\text{vero}
>\end{align}
>$$
>
>Da notare che le soluzioni sono in un intervallo definito, ma pur sempre infinito.

## Rappresentare l'insieme delle soluzioni

Si possono utilizzare:
- gli [[001 Insieme#Assegnare elementi specificando una proprietà|insiemi con proprietà]]
- gli [[004 Intervalli|intervalli]]

### Grafico per le soluzioni di una disequazione
Per risolvere in modo più semplice una disequazione di primo grado, si usa una **rappresentazione a tabella**. 

> [!warning] Attenzione
> Da non confondere con:
> - [[MB - 07 Sistemi di Disequazioni#Grafico delle soluzione del sistema di disequazioni|Grafico della soluzione di un sistema di disequazioni]]
> - [[MB - 11 Regola dei Segni per le Disequazioni#Grafico dei segni per una disequazione fratta o fattorizzata|Grafico dei segni per una disequazione fratta o fattorizzata]]

Questa è composta da:
- Una *semiretta dei reali*, contenenti dei punti che rappresentano gli estremi della soluzione
- Una *riga* al di sotto della semiretta che rappresenta, con uno o più *segmenti pieni*, l'intervallo della soluzione, indicando: 
	- Gli estremi *inclusi* ($\geq, \leq$) con un pallino pieno 
	- Gli estremi *esclusi* ($\gt,\lt$) con un pallino vuoto




![[Pasted image 20240924144004.png|500]]


Questo tipo di grafici lo si può applicare anche per l'unione di più intervalli.

> [!example]- <font color="orange">Esempio</font>
>$$\textcolor{#53ccc5}{x<2} \lor \textcolor{#FF6611}{0\leq x\leq 2}\lor \color{#e86162}x\geq 3$$
>
>Graficamente:
>![[Pasted image 20240924150038.png|500]]
>
>Intervalli:
>$$(-\infty, -2)\cup [0,2] \cup [3,+\infty)$$



---
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/disequazioni/172-disequazioni-lineari-di-primo-grado.html)