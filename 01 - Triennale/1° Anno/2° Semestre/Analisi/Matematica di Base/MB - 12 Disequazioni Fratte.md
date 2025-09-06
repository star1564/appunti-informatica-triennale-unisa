---
tags:
  - corsi/matematica/analisi
paragrafo: Matematica di Base
cssclasses:
  - 
---
>Una **Disequazione Fratta** è un particolare tipo di [[MB - 02 Disequazioni e Disuguaglianze|disequazione]] caratterizzata dalla presenza di *denominatori in cui compare l'[[MB - 00 Equazioni e Uguaglianze#Variabili (Incognite)|Incognita]]* ed è *riducibile* alla forma:
>$$\color{#CC241D}\frac{N(x)}{D(x)}\lesseqgtr0$$
>
>Questa viene detta *forma normale delle disequazioni fratte*, in cui $N(x)$ e $D(x)$ sono espressioni matematiche che dipendono dall'incognita $x$.
>
>Per essere in forma normale, l'equazione deve esserci:
>1. Un'unica frazione al primo membro
>2. Zero al secondo membro

## Metodo risolutivo delle Disequazioni Fratte
1. Proprio come [[MB - 10 Equazioni Fratte|equazioni fratte]], si specificano le [[042 Campo di Esistenza|Condizioni di Esistenza]], imponendo che ogni denominatore contenente la $x$ sia diverso da zero. $$CE:\begin{cases} \text{denominatore}_1 \neq 0 \\ \dots \\ \text{denominatore}_n \neq 0 \end{cases}$$
2. Ridurre la disequazione fratta *in forma normale* $$\frac{N(x)}{D(x)}\lesseqgtr0\quad (\star)$$ Questo lo si fa:
	- Trasportando tutti i termini a sinistra del verso della disequazione
	- Svolgendo le operazioni necessarie in modo che al primo membro si presenti un unico rapporto
3. Si **studia separatamente il segno** di numeratore e denominatore. Per farlo si pone:
	- $N(x)>0,\ D(x)>0$ se il verso della disequazione è $>$ oppure $<$
	- $N(x)\geq0,\ D(x)>0$ se il verso della disequazione è $\geq$ oppure $\leq$
4. Si studia il **segno del rapporto** attraverso la [[MB - 11 Regola dei Segni per le Disequazioni#Regola dei Segni per le disequazioni|regola dei segni per le disequazioni]], utilizzando anche il [[MB - 11 Regola dei Segni per le Disequazioni#Grafico dei segni per una disequazione fratta o fattorizzata|grafico dei segni per le disequazioni]]
5. In base al simbolo della disequazione $(\star)$ si decide di **raccogliere gli intervalli con un determinato segno**:
	- Se è $\geq$, le soluzioni della disequazione sono gli intervalli che hanno segno complessivo $+$ (con estremi inclusi)
	- Se è $\gt$, le soluzioni della disequazione sono gli intervalli che hanno segno complessivo $+$ (con estremi esclusi)
	- Se è $\leq$, le soluzioni della disequazione sono gli intervalli che hanno segno complessivo $-$ (con estremi inclusi)
	- Se è $\lt$, le soluzioni della disequazione sono gli intervalli che hanno segno complessivo $-$ (con estremi esclusi)
6. Tenere conto delle Condizioni di Esistenza attive.

> [!example]- <font color="orange">Esempio</font>
>$$\frac{x-2}{x+4}<0$$
>
>1. Trovare le condizioni di esistenza: $$CE: x+4\neq 0 \Longrightarrow x\neq -4$$
>2. Studiare il segno del numeratore e del denominatore:
>	- $x-2>0\Longrightarrow x>2$
>	- $x+4>0 \Longrightarrow x>-4$
>3. Si applica la regola dei segni attraverso il grafo dei segni per disequazioni fratte, dove si vuole prendere il segno $-$ con gli estremi esclusi, in quanto il verso della disequazione è $<$:  ![[Pasted image 20240925141915.png|500]]
>4. La soluzione è l'intervallo: $$(-4,2)$$



---
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [YouMath](https://www.youmath.it/lezioni/algebra-elementare/disequazioni/191-disequazioni-fratte.html)