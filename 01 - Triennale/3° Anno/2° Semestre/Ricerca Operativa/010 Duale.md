---
aliases:
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Programmazione Lineare
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

## Definizione
>Il **Duale** di un problema di [[007 Programmazione Lineare|Programmazione Lineare]] è un altro problema di PL che è *derivato dal problema originale* (chiamato **primale**). Accade che:
>- Ogni variabile nel primale diventa un vincolo nel duale
>- Ogni vincolo nel primale diventa una variabile nel duale
>- La "direzione" della funzione obbiettivo è invertita - Il massimo nel primale diventa minimo nel duale e viceversa

![[Pasted image 20240710121512.png]]

### Teorema Forte della dualità
Data una coppia di problemi primale duale, $(P)$ e $(D)$, se uno dei due problemi ammette una soluzione ottima finita, allora anche l'altro problema ammette una soluzione ottima finita ed i valori ottimi delle funzioni obiettivo coincidono. Ovvero $$\underline{c}^T\ \underline{x}^* = \underline{b}^T\ \underline{w}^*$$


## Risoluzione
### Tabella
Si scrivono prima i vincoli in orizzontale e poi si leggono in verticale.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240710121618.png]]

### Segni dei vincoli
Le variabili nel "vincolo finale" (ad esempio $x_1\geq0, x_2=0,x_3\leq0$) modificano il segno del duale.
Ci sono tre segni:
- $\leq$
- $\geq$
- $\text{n.v.}$ (non variante)che equivale a $=$

Per effettuare la trasformazione:
- 🔁 Per ogni variabile "vincolo finale" $i$ del primale
	- Il vincolo $i$-esimo del duale sarà:
		- $\geq$, se il vincolo finale era $\leq$
		- $\leq$, se il vincolo finale era $\geq$
		- $=$, se il vincolo finale era $\text{n.v.}$
- 🔁 Per ogni vincolo $j$ del primale
	- Il "vincolo finale" $j$-esimo del duale sarà:
		- $\geq$, se il vincolo finale era $\geq$
		- $\leq$, se il vincolo finale era $\leq$
		- $=$, se il vincolo finale era $\text{n.v.}$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240710124043.png]]


### Soluzione ottima del duale
Data la base ottima $B$ del primale, si può calcolare la soluzione ottima del duale. Si fa riferimento al [[#Teorema Forte della dualità|Teorema Forte della dualità]].

Se si conosce una base ammissibile ed ottima per un primale, allora possiamo usare la seguente formula per trovare una soluzione ammissibile e ottimale per il duale:
$$\underline{w}^* = \underline{c}^T_B\ A_B^{-1}$$

> [!example]- <font color="orange">Esempio</font>
>Avendo il problema [[009 Simplesso#^5b4d46|di prima]], abbiamo che $B=\{1, 3, 6\}$ è una base ammissibile e ottima.
>
>$$\underline{w}^*=
>\begin{bmatrix}
>\frac{1}{5} & \frac{3}{5} & 0\\
>\frac{2}{5} & \frac{1}{5} & 0\\
>-\frac{1}{5} & \frac{7}{5} & 1\\
>\end{bmatrix}
>\begin{bmatrix}
>-4 & -2 & 0
>\end{bmatrix}=
>\begin{bmatrix}
>-\frac{8}{5} \\
>-\frac{14}{5} \\
>0
>\end{bmatrix}
>$$
>
>Questa è la soluzione ammissibile e ottima per il duale.

