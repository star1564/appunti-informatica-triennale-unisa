---
aliases: 
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Programmazione Lineare
cssclasses:
  - 
---

> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

È un algoritmo per calcolare la soluzione ottimale di un problema di programmazione lineare in [[Forma standard di un problema di PL|forma standard]].

---
> [!error] ATTENZIONE
> Questo NON è il metodo delle due fasi che chiede all'esame.


Bisogna trasformare il problema in *forma standard*. Si usano questi termini:

|                            | Descrizione                                                                                     |
| :------------------------: | ----------------------------------------------------------------------------------------------- |
|            $n$             | Il numero di variabili dei vincoli                                                              |
|            $m$             | Il numero di vincoli                                                                            |
|            $A$             | La matrice $m\times n$ del sistema dei coefficienti dei vincoli                                 |
|           $A_B$            | La sottomatrice composto delle colonne in base di $A$                                           |
|         $A_B^{-1}$         | La matrice inversa di $A_B$                                                                     |
|      $\underline{b}$       | Il vettore dei termini noti dei vincoli                                                         |
|     $\underline{a}_k$      | La $k$-esima colonna di $A$                                                                     |
|     $\underline{c}^T$      | Il vettore dei coefficienti della funzione obbiettivo                                           |
|           $c_j$            | Il $j$-esimo elemento di $\underline{c}^T$                                                      |
|            $B$             | Un insieme di cardinalità $m$, che contiene l'indice delle colonne in base che compongono $A_B$ |
|            $N$             | Un insieme che contiene l'indice delle colonne non in base                                      |
|           $z_j$            | Il costo ridotto della variabile $x_j$                                                          |
|           $x_k$            | La variabile entrante per questa iterazione                                                     |
|           $x_r$            | La variabile uscente per questa iterazione                                                      |
|     $\underline{y}_k$      | Il vettore direzionale associato alla variabile entrante $x_k$                                  |
| $\underline{\overline{b}}$ | Il vettore della soluzione corrente della base                                                  |
|      $\overline{b}_j$      | Il $j$-esimo componente di $\underline{\overline{b}}$                                           |
|         $y_{j\ k}$         | Il $j$-esimo componente di $\underline{y}_k$                                                    |

Inserire nei vincoli una variabile di slack ($-$ se il vincolo è $\geq$, $+$ se il vincolo è $\leq$).

> [!warning] Preferire tutti i vincoli $\leq$
> Se ci sono dei vincoli $\geq$, invertirli per trasformarli in $\leq$. In questo modo si avranno delle variabili di slack positive.

Scegliere come base di partenza la matrice identità data dalle colonne delle variabili di slack.

### 1. Test di Ottimalità


Calcolare:
$$z_j-c_j=\underline{c}^T_B\ A_B^{-1}\ \underline{a}_j-c_j,\quad \forall j \in N$$
Ci sono due possibilità:
- Se $\textcolor{#61AFEF}{z_j-c_j\leq 0}, \color{#CC241D}\forall j\in N$, allora la base corrente è **ottima** e l'algoritmo termina ✔️
- Altrimenti, si va al punto 2⬇️

### 2. Scelta della variabile entrante in base
Scegliere una variabile $x_k$ fuori base $(k\in N)$ tale che $z_k-c_k>0$. Se ci sono molteplici contendenti, scegliere il primo che si è trovato. 

Andare al punto 3⬇️


### 3. Test di illimitatezza
Calcolare questi due valori:
$$\underline{y}_k=A_B^{-1}\ \underline{a}_k$$
$$\underline{\overline{b}}=A_B^{-1}\ \underline{b}$$

Ci sono due possibilità:
- Se $y_{k}$ ha *tutte le componenti* $\textcolor{#61AFEF}{\leq 0}$, allora l'**Ottimo è illimitato** (non esiste ottimo finito) e l'algoritmo termina ❌
- Se *esiste almeno una componente* $\textcolor{#61AFEF}{> 0}$ in $y_{k}$, si va al punto 4⬇️

### 4. Scelta della variabile uscente dalla base (test dei minimi rapporti)

Si determina quale variabile $(x_r)$ attualmente in base dovrà uscire per fare spazio alla variabile entrante $(x_k)$ scelta nel punto 2.

$$x_r=\frac{\overline{b}_r}{y_{r\ k}}=\min_{j\in B}\bigg\{\frac{\overline{b}_j}{y_{j\ k}}:y_{j\ k}>0\bigg\}$$

In pratica si procede in questo modo:
- Individua tutti i componenti $>0$ di $y_k$
- 🔁 Per ognuno di questi, segna la posizione $j$ ed aggiungi all'insieme in cui trovare il minimo questa frazione:
	- Numeratore: il $j$-esimo componente di $\underline{\overline{b}}$
		- Segna, inoltre, la variabile $r\in B$ di questo componente
	- Denominatore: il $j$-esimo componente di $\underline{y}_k$
- Trova il minimo in questo insieme
- La variabile uscente $x_r$ sarà la $r$ segnata in precedenza su $\overline{b}_j$
- La variabile entrante $x_k$ sarà la $k$ di $y_k$

> [!warning] Le variabili $r$ dipendono dal contenuto di $B$
> Se $B=\{1, 3, 5\}$ e $j=2$ allora, con $\overline{b}_j$ si ha che: $r=3$, $x_r=x_3$.



### 5. Aggiornamento della base
Aggiornare gli indici delle variabili in base e fuori base: 
- Sostituisci la variabile uscente $x_r$ in $B$ con la variabile entrante $x_k$
- Sostituisci la variabile entrante $x_k$ in $N$ con la variabile uscente $x_r$

Tornare al punto 1⤴️

### (Opzionale) Verifica ammissibilità della base
Si può verificare che una base sia **ammissibile** calcolando $$\underline{x}_b=A_B^{-1}\ \underline{b}$$

Se questo ha tutti i componenti $\color{#61AFEF}\geq 0$, allora la base è ammissibile.

---
> [!example]- <font color="orange">Esempio PDF</font>
>![[Simplesso_annotato3.pdf]]

^5b4d46
