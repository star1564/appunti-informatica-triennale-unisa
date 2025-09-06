---
aliases:
  - formula quadratica
  - fattorizzazione di polinomi
tags:
  - corsi/matematica/analisi
paragrafo: Matematica di Base
cssclasses:
  - 
---
>Un'**Equazione di secondo grado** in *una incognita* è un'[[MB - 00 Equazioni e Uguaglianze|equazione]] *riducibile* alla forma:
>$$\textcolor{#00C575}{a}\textcolor{#FF6611}{x^2}+\textcolor{#00C575}{b}\textcolor{#FF6611}{x}+\textcolor{#00C575}{c}=0, \quad \textcolor{#00C575}{a}\neq 0$$
>
>Questa viene detta *forma normale delle equazioni di secondo grado*, in cui: 
>- [[MB - 00 Equazioni e Uguaglianze#Costanti|Costanti]] coefficienti:
>	- $\textcolor{#00C575}{a}$ è il coefficiente del termine di grado 2 
>	- $\textcolor{#00C575}{b}$ è il coefficiente del termine di grado 1
>	- $\textcolor{#00C575}{c}$ è il termine noto
>- $\textcolor{#FF6611}{x}$ è l'[[MB - 00 Equazioni e Uguaglianze#Variabili (Incognite)|incognita]], oggetto della risoluzione dell'equazione di secondo grado, di cui bisogna individuare i valori che rendono vera l'uguaglianza
>
>Un'equazione è di secondo grado se in essa, l'incognita $\textcolor{#FF6611}{x}$ appare:
>- Almeno una volta con *esponente 2* (grado 2)
>- Con *grado massimo pari a 2*

> [!example]+ <font color="orange">Esempio</font>
>- $x^2+5x+4=0$
>- $x^2=4x$
>- $x^2=1$
>- $x-x^2=4+x-x^2$
>- $\dots$

Da notare che se $a=0$, si ha una [[MB - 04 Equazioni di Primo Grado|equazione di primo grado]].

Lo *studio delle equazioni di secondo grado* ha lo scopo di riuscire a risolvere qualsiasi equazione di secondo grado in forma normale o, eventualmente, qualsiasi equazione che sia stata ricondotta alla precedente forma normale.

## Risolvere Equazioni di Secondo Grado
### Formula quadratica
Questo è il **metodo generale** per risolvere le equazioni di secondo grado *in forma normale*.

>Esso prevede di considerare una quantità che è *caratteristica delle equazioni di secondo grado*, chiamato **discriminante** (o *delta*)
>$$\color{#CC241D}\Delta=b^2-4ac$$
>
>Il discriminante ci permette di scrivere la formula risolutiva:
>$$x_{1,2}=\frac{-b\ \pm\ \sqrt{\Delta}}{2a}=\textcolor{#CC241D}{\frac{-b\ \pm\ \sqrt{b^2-4ac}}{2a}} \Rightarrow \begin{cases} x_1=\frac{-b\ +\ \sqrt{b^2-4ac}}{2a} \\ x_2=\frac{-b\ -\ \sqrt{b^2-4ac}}{2a} \end{cases}$$

Le soluzioni $x_1$ e $x_2$ sono chiamate **radici** dell'equazione.
#### Possibili soluzioni per la formula risolutiva
1. Se $\color{#CC241D}\Delta>0$, l'equazione è **determinata** ed *ammette due soluzioni reali*. Ovvero che: 
   $$S=\{x_1,x_2\},\quad x_1,x_2\in\mathbb{R}, \quad x_1\neq x_2$$
   
   ![[Pasted image 20220303155703.png|400]]

2. Se $\color{#CC241D}\Delta=0$, l'equazione è **determinata** ed *ammette una soluzione reale*. Ovvero che le due soluzioni coincidono, quindi: $$S=\{x_1\},\quad x_1\in\mathbb{R}$$
   
   ![[Pasted image 20220303155908.png|400]]
   
3. Se $\color{#CC241D}\Delta<0$, l'equazione *non ha soluzioni* ed è **impossibile**. Ovvero che: $$\not\exists x\in\mathbb{R}$$
   
   ![[Pasted image 20220303160047.png|400]]
### Risoluzione con Fattorizzazione
Per risolvere le equazioni di secondo grado tramite il metodo della **fattorizzazione**, si scompone l'equazione *dalla forma standard* in fattori, ovvero
$$(\text{fattore 1})(\text{fattore 2})=0$$

Si seguono questi passi:


1. Calcolare il prodotto del coefficiente $a$ e del termine noto: $$\color{#CC241D}a\cdot c=p$$
2. Trovare due numeri $u,v$ che:
	- *Moltiplicati diano $p$*
	- *Sommati (o sottratti) diano il coefficiente $b$*


> [!tip] Questi due numeri si trovano tra i fattori di $p$


3. Usare i due numeri trovati per *riscrivere il termine centrale*, dividendolo in due parti. Ovvero: $$ax^2 + \textcolor{#61AFEF}{ux + vx} + c=0$$

4. Raggruppare i termini in *due coppie*, dove la prima è composta da i primi due numeri, mentre la seconda dagli ultimi due. Ovvero: $$x(x+u)+(vx+c)=0$$

5. Estrarre un *fattore comune* da ciascuna coppia. Se i fattori sono corretti, si sarà in grado di scrivere l'equazione come il *prodotto di due binomi*, ciascuno uguale a zero.

6. Poiché il prodotto è uguale a zero, si impone che *ciascun binomio sia uguale a zero* e si risolve per $x$.

> [!example]- <font color="orange">Esempio</font>
>$$2x^2 + 7x + 3=0$$
>
>1. $p=a\cdot c=2\cdot3=6$
>2. $u=6, v=1$
>	- Fattori di $p=6\Longrightarrow \textcolor{#61AFEF}6,3,2,\textcolor{#61AFEF}1$
>3. $2x^2 +\textcolor{#61AFEF}{6x + x} + 3=0$
>4. $(2x^2+6x)+(x+3)=0\Longrightarrow \textcolor{#FF6611}{2x}\textcolor{#00C575}{(x+3)}+ \textcolor{#FF6611}{1}\textcolor{#00C575}{(x+3)}=0$
>5. Il fattore comune è $(x+3)$, quindi $\textcolor{#FF6611}{(2x+1)}\textcolor{#00C575}{(x+3)}=0$
>6. $\begin{cases} 2x+1=0 \\ x+3=0 \end{cases}\Longrightarrow \begin{cases} x_1= \frac{1}{2} \\ x_2=-3 \end{cases}$

## Scomposizione in Fattori

>Se il $\color{#CC241D}\Delta\geq 0$, allora è possibile fattorizzare il polinomio di secondo grado $\textcolor{#00C575}{a}x^2+bx+c$ in questo modo:
>$$\color{#CC241D}\textcolor{#00C575}{a}(x-\textcolor{#61AFEF}{x_1})(x-\textcolor{#61AFEF}{x_2})$$
>Dove $\color{#61AFEF}x_1,x_2$ sono le [[#Formula quadratica|soluzioni]] del polinomio.

> [!example]- <font color="orange">Esempio</font>
>$$x^2+5x+6$$
>$$\Delta=25-(4\cdot1\cdot 6)=25-24=1 >0$$
>- $a=1$
>- $x_1=-3$
>- $x_2=-2$
>
>Scomposizione:
>$$1(x-(-3))(x-(-2))=\color{#CC241D}(x+3)(x+2)$$

---
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/equazioni/68-equazioni-di-secondo-grado-ad-unincognita-come-si-risolvono.html)
- https://www.mathsisfun.com/algebra/factoring-quadratics.html