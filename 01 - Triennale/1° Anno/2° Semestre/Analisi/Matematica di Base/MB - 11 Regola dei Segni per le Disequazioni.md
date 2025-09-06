---
aliases:
  - Disequazioni di grado superiore al secondo
tags:
  - corsi/matematica/analisi
paragrafo: Matematica di Base
cssclasses:
  - 
---
## Regola dei Segni per le disequazioni
>Il metodo della **regola dei segni per le disequazioni** prevede di determinare le soluzioni, ove possibile, studiando i *segni dei singoli fattori* che compaiono nella [[MB - 02 Disequazioni e Disuguaglianze|disequazione]], e individuando il *segno complessivo* mediante un'apposita rappresentazione tabellare.
>
>Si può applicare quando si ha:
>- Una disequazione di grado superiore al secondo *scomposta in diversi fattori* $$(\text{fattore 1})\cdot\ldots\cdot(\text{fattore }n)\lesseqgtr 0$$
>- Una [[MB - 12 Disequazioni Fratte|disequazione fratta]] $$\frac{N(x)}{D(x)}\lesseqgtr0$$

Qui si parlerà dell'utilizzo della regola dei segni per una disequazione di grado superiore al secondo, in quanto il processo finale è abbastanza simile.

## Metodo risolutivo
Supponiamo di avere la seguente disequazione di grado superiore al secondo scritta in forma normale. $$P(x)\lesseqgtr 0$$
1. Si *fattorizza il polinomio* $P(x)$ con un qualsiasi metodo di scomposizione. Supponiamo che si scomponga in $k$ fattori: $$P_1(x)\cdot\ldots\cdot P_k(x) \lesseqgtr 0\quad (\star)$$
2. Si studiano *separatamente* il segno dei *singoli fattori*. Si hanno due casi, ma in entrambi si andranno a risolvere disequazioni di [[MB - 05 Disequazioni di Primo Grado|primo]] o di [[MB - 09 Disequazioni di Secondo Grado|secondo]] grado:
	- Se in $(\star)$ si ha come simbolo di disequazione un *$\geq$ oppure un $\leq$*, allora si studia il segno dei singoli fattori **ponendoli sempre a $\geq$**.
	  $$\begin{align}P_1(x) &\geq 0\\ &\dots \\ P_k(x) &\geq 0\end{align}$$
	- Se in $(\star)$ si ha come simbolo di disequazione un *$\gt$ oppure un $\lt$*, allora si studia il segno dei singoli fattori **ponendoli sempre a $\gt$**.
	  $$\begin{align}P_1(x) &\gt 0\\ &\dots \\ P_k(x) &\gt 0\end{align}$$

3. Viene applicata la *regola dei segni*, usando le informazioni sui segni dei singoli fattori per risalire al segno del loro prodotto, ovvero il polinomio $P(x)$. Si può anche utilizzare il grafico dei segni.
4. In base al simbolo della disequazione $(\star)$ si decide di **raccogliere gli intervalli con un determinato segno**:
	- Se è $\geq$, le soluzioni della disequazione sono gli intervalli che hanno segno complessivo $+$ (con estremi inclusi)
	- Se è $\gt$, le soluzioni della disequazione sono gli intervalli che hanno segno complessivo $+$ (con estremi esclusi)
	- Se è $\leq$, le soluzioni della disequazione sono gli intervalli che hanno segno complessivo $-$ (con estremi inclusi)
	- Se è $\lt$, le soluzioni della disequazione sono gli intervalli che hanno segno complessivo $-$ (con estremi esclusi)
5. Tenere conto delle [[042 Campo di Esistenza|Condizioni di Esistenza]] attive.

### Grafico dei segni per una disequazione fratta o fattorizzata

> [!warning] Attenzione
> Da non confondere con:
> - [[MB - 05 Disequazioni di Primo Grado#Grafico per le soluzioni di una disequazione|Grafico per le soluzioni di una disequazione di primo grado]]
> - [[MB - 07 Sistemi di Disequazioni#Grafico della soluzione del sistema di disequazioni|Grafico della soluzione del sistema di disequazioni]]

Questo è composto da:
- Una *semiretta dei reali*, contenenti dei punti che rappresentano gli estremi della soluzione di *ciascuna disequazione*
- *Tante righe quante sono le disequazioni del sistema* al di sotto della semiretta dei reali, dove ogni riga rappresenta l'intervallo della soluzione della relativa disequazione, si disegna:
	- Con un *segmento pieno*, gli intervalli in cui il rispettivo fattore è positivo
	- Con un *segmento tratteggiato*, gli intervalli al di fuori dei segmenti pieni
- Inoltre, su ogni riga si indicano: 
	- Gli estremi *inclusi* ($\geq, \leq$) con un pallino pieno 
	- Gli estremi *esclusi* ($\gt,\lt$) con un pallino vuoto

![[Pasted image 20240925133716.png|1000]]

Dopo aver creato il grafico, si applica la regola dei segni, **analizzando la rappresentazione tabellare** guardando i vari intervalli in *verticale* e si *conta il numero di righe tratteggiate* su ciascun intervallo:
- Se c'è un numero di *righe tratteggiate pari*, o se non ce ne sono, allora il prodotto complessivo per quell'intervallo è **positivo**
- Se c'è un numero di *righe tratteggiate dispari*, allora il prodotto complessivo per quell'intervallo è **negativo**

> [!example]- <font color="orange">Esempio</font>
>$$x^3-2x^2+x<0$$
>
>1. Questa disequazione può essere scomposta in due fattori: $$x(x^2-2x+1)<0$$
>2. Si studiano i singoli fattori. Dato che il segno della disequazione è $<$, questi verranno posti con $>0$:
>	- **Primo fattore** ([[MB - 05 Disequazioni di Primo Grado|disequazione di primo grado]] già risolta): $x>0$
>	- **Secondo fattore** ([[MB - 09 Disequazioni di Secondo Grado|disequazione di secondo grado]]): $x^2-2x+1>0 \Longrightarrow \forall x \in \mathbb{R}, x\neq 1$
>
>3. Si rappresenta la tabella per il confronto dei segni. ![[Pasted image 20240925134540.png|500]]
>4. Dato che il verso della disequazione è $<$, si prendono gli intervalli in cui il prodotto è negativo. Quindi la soluzione per questa disequazione è $$(-\infty, 0)$$

---
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/disequazioni/2320-regola-dei-segni-per-le-disequazioni.html)
- [YouMath - Disequazioni di grado superiore al secondo](https://www.youmath.it/lezioni/algebra-elementare/disequazioni/206-disequazioni-di-grado-superiore-al-secondo.html)