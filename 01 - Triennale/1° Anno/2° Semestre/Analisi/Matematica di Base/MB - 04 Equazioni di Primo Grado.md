---
tags:
  - corsi/matematica/analisi
paragrafo: Matematica di Base
cssclasses:
  - 
---
>Un'**Equazione di primo grado** in *una incognita* è un'[[MB - 00 Equazioni e Uguaglianze|equazione]] *riducibile* alla forma:
>$$\textcolor{#00C575}{a}\textcolor{#FF6611}{x}+\textcolor{#00C575}{b}=0$$
>
>Questa viene detta *forma normale delle equazioni di primo grado*, in cui: 
>- $\textcolor{#00C575}{a}$ e $\textcolor{#00C575}{b}$ sono delle [[MB - 00 Equazioni e Uguaglianze#Costanti|costanti]] coefficienti
>- $\textcolor{#FF6611}{x}$ è l'[[MB - 00 Equazioni e Uguaglianze#Variabili (Incognite)|incognita]], oggetto della risoluzione dell'equazione di primo grado, di cui bisogna individuare i valori che rendono vera l'uguaglianza
>
>In questo tipo di equazione, l'incognita $\textcolor{#FF6611}{x}$ è elevata a *esponente 1* (grado 1), può essere solamente moltiplicata per un coefficiente $\textcolor{#00C575}{a}$ qualsiasi e può essere sommata ad un qualsiasi coefficiente $\textcolor{#00C575}{b}$.

> [!example]+ <font color="orange">Esempio</font>
>- $x+5=0$
>- $x+19=4$
>- $x+5=7+3x$
>- $\dots$

## Possibili soluzioni
1. Se *non si ha alcuna soluzione*, si dice che l'equazione è **Impossibile**. Ovvero se $$a=0\land b\neq 0$$
   In questo caso si scrive che $\color{#61AFEF}\not\exists x\in \mathbb{R}$.
   
   ![[Pasted image 20240924122256.png|450]]
    ^55783a
2. Se si ha *un numero infinito di soluzioni*, si dice che l'equazione è **Indeterminata**. Ovvero se $$a=0 \land b=0$$
   In questo caso si scrive che $\color{#61AFEF}\forall x\in \mathbb{R}$.
   
   ![[Pasted image 20240924122510.png|450]]
   
   
3. Se non si rientra nei casi precedenti, *nel caso di un'equazione di primo grado*, si ha **una sola soluzione**. Ovvero se $$a\neq 0 \iff x= \frac{b}{a}$$
   Si scrive che $\color{#61AFEF}x=\text{valore}$.
   
   ![[Pasted image 20240924122218.png|450]]

## Risolvere Equazioni di Primo Grado
Si possono sfruttare i [[MB - 01 Principi di Equivalenza delle Equazioni|Principi di Equivalenza]] per ridurre un'equazione alla sua forma normale. 

> [!tip] Suggerimento
> Spostare tutte le incognite al primo membro e tutte le costanti al secondo membro.
> $$\text{incognite}=\text{costanti}$$


Da cui possiamo notare che: 

- Se dobbiamo **spostare un elemento** da un membro all'altro, *questo si sposta e cambia segno*.

> [!example]- <font color="orange">Esempio</font>
>$$
>\begin{aligned}
>x\textcolor{#00C575}{+5}  &=  0 \\
>x+5\textcolor{#CC241D}{-5}  &=  0 \textcolor{#CC241D}{-5} \\
>x  &=  \textcolor{#00C575}{-5} \\
>\end{aligned}
>$$

- Se dobbiamo **cambiare il segno di un membro**, *si cambia anche il segno dell'altro membro*.

> [!example]- <font color="orange">Esempio</font>
>$$
>\begin{aligned}
>\textcolor{#00C575}{-x} &=  1 \\
>\textcolor{#CC241D}{-1}(-x) &=  \textcolor{#CC241D}{-1}(1) \\
>\textcolor{#00C575}{x} &=  -1 \\
>\end{aligned}
>$$


> [!example]+ <font color="orange">Esempio di risoluzione e di prova</font>
>$$
>\begin{aligned}
>x+5  &=  7+3x \\
>x+5\textcolor{#CC241D}{-5}  &=  7\textcolor{#CC241D}{-5}+3x \\
>x  &=  2+3x \\
>x\textcolor{#CC241D}{-3x}  &=  2+3x\textcolor{#CC241D}{-3x} \\
>-2x&=2 \\
>-\textcolor{#CC241D}{\frac{2}{2}}x&=\textcolor{#CC241D}{\frac{2}{2}} \\
>-x&=1 \\
>\textcolor{#CC241D}{-1}(-x)&=\textcolor{#CC241D}{-1}(1) \\
>x&=-1 \\
>\end{aligned}
>$$
>
>Sostituendo $x$ a $-1$ nell'equazione originale otteniamo
>$$
>\begin{aligned}
>-1+5&=7+3(-1) \\
>4&=7+-3 \\
>4&=4 \\
>&\text{vero}
>\end{aligned}
>$$
>Dato che l'uguaglianza è vera, si è provato che $x=-1$ è la soluzione all'equazione.


---
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/equazioni/44-equazioni-di-i-grado-ad-una-incognita-prima-parte.html)