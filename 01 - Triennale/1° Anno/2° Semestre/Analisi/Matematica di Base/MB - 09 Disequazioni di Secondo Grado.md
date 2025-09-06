---
tags:
  - corsi/matematica/analisi
paragrafo: Matematica di Base
cssclasses:
  - 
---
>Una **Disequazione di secondo grado** in *una incognita* è una [[MB - 02 Disequazioni e Disuguaglianze#Disequazione|disequazione]] *riducibile* alla forma:
>$$\textcolor{#00C575}{a}\textcolor{#FF6611}{x^2}+\textcolor{#00C575}{b}\textcolor{#FF6611}{x}+\textcolor{#00C575}{c}\lesseqgtr0, \quad \textcolor{#00C575}{a}\neq 0$$
>
>Questa viene detta *forma normale delle disequazioni di secondo grado*, in cui: 
>- [[MB - 00 Equazioni e Uguaglianze#Costanti|Costanti]] coefficienti:
>	- $\textcolor{#00C575}{a}$ è il coefficiente del termine di grado 2 
>	- $\textcolor{#00C575}{b}$ è il coefficiente del termine di grado 1
>	- $\textcolor{#00C575}{c}$ è il termine noto
>- $\textcolor{#FF6611}{x}$ è l'[[MB - 00 Equazioni e Uguaglianze#Variabili (Incognite)|incognita]], oggetto della risoluzione della disequazione di secondo grado, di cui bisogna individuare i valori che rendono vera la disuguaglianza
>
>Una disequazione è di secondo grado se in essa, l'incognita $\textcolor{#FF6611}{x}$ appare:
>- Almeno una volta con *esponente 2* (grado 2)
>- Con *grado massimo pari a 2*

> [!example]+ <font color="orange">Esempio</font>
>- $x^2-3x+1\geq 0$
>- $x^2-2x\leq 1$
>- $5x^2+7<0$
>- $2x^2+3x>-3x^2+1$
>- $\dots$

## Risolvere Disequazioni di Secondo Grado
La disequazione deve essere in forma normale:
1. Il coefficiente *$a$ deve essere positivo*. Quindi se $a$ è negativo bisogna cambiare tutti i segni, [[MB - 03 Principi di Equivalenza delle Disequazioni#^da6cff|incluso il verso della disequazione]].
2. Risolvere l'[[MB - 08 Equazioni di Secondo Grado#Risolvere Equazioni di Secondo Grado|equazione di secondo grado]] associata $$ax^2+bx+c=0$$
3. Una volta ottenuto il risultato (due soluzioni, una soluzione, nessuna soluzione), si esegue come di seguito.
   
> [!warning] Attenzione
> Il verso della disequazione da considerare è quello della disequazione attuale. 
> Ovvero, se si è cambiato il verso della disequazione, allora il verso da considerare sarà quello nuovo.

### Due soluzioni ($\Delta>0$)

$$S=\{x_1,x_2\},\quad x_1\neq x_2$$
Supponendo per semplicità che $x_1<x_2$:

| Verso della disequazione |         Soluzione         |
| :----------------------: | :-----------------------: |
|   $\color{#00C575}\gt$   |     $x<x_1\lor x>x_2$     |
|  $\color{#00C575}\geq$   | $x\leq x_1\lor x\geq x_2$ |

![[Pasted image 20220303160827.png|400]]

| Verso della disequazione |      Soluzione      |
| :----------------------: | :-----------------: |
|   $\color{#FF6611}\lt$   |     $x_1<x<x_2$     |
|  $\color{#FF6611}\leq$   | $x_1\leq x\leq x_2$ |

![[Pasted image 20220303161218.png|400]]

### Una soluzione ($\Delta=0$)
Ovvero due soluzioni reali ma coincidenti.
$$S=\{x_1,x_2\}=\{x_1\},\quad x_1=x_2$$

| Verso della disequazione |              Soluzione              |
| :----------------------: | :---------------------------------: |
|    $\color{#00C575}>$    | $\forall x\in\mathbb{R}, x\neq x_1$ |
|  $\color{#00C575}\geq$   |      $\forall x\in\mathbb{R}$       |
|    $\color{#FF6611}<$    |             impossibile             |
|  $\color{#FF6611}\leq$   |      unica soluzione: $x=x_1$       |

### Nessuna soluzione ($\Delta<0$)

$$S=\emptyset$$

| Verso della disequazione |        Soluzione         |
| :----------------------: | :----------------------: |
|    $\color{#00C575}>$    | $\forall x\in\mathbb{R}$ |
|  $\color{#00C575}\geq$   | $\forall x\in\mathbb{R}$ |
|    $\color{#FF6611}<$    |  non ammette soluzioni   |
|  $\color{#FF6611}\leq$   |  non ammette soluzioni   |

> [!example]- <font color="orange">Esempio</font>
>$$-3x^2+5x-2\geq 0$$
>Il coefficiente $a$ è negativo, si cambia il segno:
>$$3x^2-5x+2\leq 0$$
>
>Si risolve l'equazione di secondo grado associata: $$3x^2-5x+2\leq 0$$
>$$\Delta = b^2 -4ac = (-5)^2 - 4\cdot 3\cdot 2= 25-24 = 1$$
>$$x_{1,2}= \frac{-b\pm \sqrt{\Delta}}{2a}= \frac{5\pm 1}{6}=\begin{cases} x_1 = 1 \\ x_2 = \frac{2}{3} \end{cases}$$
>
>Il segno della disequazione è $\le$, quindi la soluzione è $$\frac{2}{3}\leq x\leq 1$$


---
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/disequazioni/174-disequazioni-di-secondo-grado-quadratiche.html)