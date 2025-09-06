---
aliases:
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Problemi su Grafi
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

>L'**algoritmo dei cammini aumentanti** risolve il [[012 Problema del Massimo Flusso|Problema del Massimo Flusso]] utilizzando il [[012 Problema del Massimo Flusso#Grafo Ausiliario|grafo ausiliario]] per stabilire come instradare il flusso sulla rete.

Un **cammino aumentato** è un cammino da $s$ a $t$ sul grafo ausiliario.

## Risoluzione
Consideriamo un grafo $G=(V,E)$ ed un flusso ammissibile $\underline{x}$. Inizialmente il metodo considera il flusso nullo, ossia $$x_{i,j}=0, \forall(i,j) \in E$$
I passi principali dell'algoritmo dei cammini aumentanti sono:
1. Imposta inizialmente $$\Delta=f=0$$
2. Individuare nel grafo ausiliario un qualsiasi cammino $p$ *dal nodo sorgente al nodo pozzo* su cui è possibile far transitare una quantità di flusso $\Delta>0$ (cammino aumentante). 
	- Se non esiste tale cammino, l'algoritmo si arresta. ✔️
3. Il valore del flusso da inviare lungo il cammino $p$ è pari alla *capacità residua minima degli archi in $p$*. Ovvero $$\Delta=\min\{r_{i,j}:(i,j) \in p\}$$
4. Incrementare di $\Delta$ il valore del flusso $f$ corrente, quindi $f=f+\Delta$, e aggiornare le capacità residue degli archi lungo il cammino $p$ nel seguente modo: $$r_{i,j}= r_{i,j} - \Delta,\quad r_{j,i}= r_{j,i} + \Delta$$
5. Ritorna al punto 2⤴️

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240711103940.png]]
>![[Pasted image 20240711104024.png]]
>![[Pasted image 20240711104051.png]]
>![[Pasted image 20240711104535.png]]
>![[Pasted image 20240711104607.png]]
>![[Pasted image 20240711104637.png]]


### Valore delle variabili decisionali
Il valore delle variabili decisionali, per ogni arco del grafo di partenza, è pari alla differenza tra la capacità originale dell'arco meno quella residua nell'ultimo grafo ausiliario (se tale valore è negativo la variabile decisionale varrà zero). Ovvero
$$x_{i,j} = \max\{0 , u_{i,j} - r_{i,j}\}$$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240711104911.png]]
>
>$x_{1,2}=5 - 2=3$
>$x_{1,5}=8 - 1=7$
>$x_{1,3}=4 - 0=4$
>$x_{2,5}=1 - 1=0$
>$x_{2,4}=3 - 0=3$
>$x_{3,6}=5 - 1=4$
>$x_{4,2}=\max\{0, 2 - 5\}=0$
>$x_{4,6}=\max\{0, 1 - 2\}=0$
>$x_{4,7}=10 - 2=8$
>$x_{5,3}=3 - 3=0$
>$x_{5,4}=4 - 0=4$
>$x_{5,6}=6 - 3=3$
>$x_{6,4}=1 - 0=1$
>$x_{6,7}=6 - 0=6$
>$x_{7,6}=\max\{0, 1 - 7\}=0$

### Individuazione del Taglio Minimo
Per poter individuare il taglio minimo, la cui capacità sarà uguale al flusso massimo $f$, è sufficiente *controllare quali sono i nodi raggiungibili dalla sorgente* attraverso archi con *capacità residua $>0$* nell'ultimo grafo ausiliario.

> [!tip] Suggerimento
> - Dato un arco $(u,v)$, ignorare gli archi che hanno uno 0 su $u$.
> - Se un arco ha un numero $\neq 0$ su $u$, ignorarlo se la direzione della freccia nel grafo originale va verso $u$.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240711111952.png]]