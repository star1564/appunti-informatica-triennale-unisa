---
aliases:
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Problemi su Grafi
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

Sia $G = (V,E)$ un grafo orientato su cui sia definito un vettore $u = [u_{i\ j}]$ delle capacità associate agli archi del grafo; inoltre, siano $s$ e $t$ due nodi distinti, detti rispettivamente sorgente (o origine) e pozzo (o destinazione). 

>Il **problema del flusso massimo** consiste nel determinare la *massima quantità di flusso* che è possibile inviare *da $s$ a $t$* attraverso $G$.


![[Pasted image 20240711093515.png|800]]

Si ha che:
- Un nodo sorgente fornisce flusso positivo $f$
- Un nodo destinazione fornisce flusso negativo $-f$
- Tutti gli altri nodi sono nodi di transito

### Modello matematico
$$\max f$$

Con i seguenti vincoli
$$\boxed{1}\quad\displaystyle\sum_{j\in FS(i)} x_{i, j}-\displaystyle\sum_{k\in BS(i)} x_{k, i}=\begin{cases} 0\quad \forall i\in V, i\neq s,t \\ f\quad\text{se }i=s \\ -f\quad\text{se }i=t \end{cases}$$
$$\boxed{2}\quad0\le x_{i,j}\le u_{i,j}\quad \forall(i,j)\in E$$

Un **flusso ammissibile** è un cammino da $s$ in $t$ che non viola i vincoli di capacità.

### Taglio
Un **taglio** è un *partizionamento dei vertici* in due sottoinsiemi $V_1$ e $V_2$, tali che:
- Il nodo sorge appartiene a $V_1$
- Il nodo pozzo appartiene a $V_2$
- $V_1\cup V_2=V$
- $V_1\cap V_2=\emptyset$

![[Pasted image 20240711095422.png]]

Un arco $(u,v)$ può essere:
- **Diretto**, se $u\in V_1$ e $v\in V_2$
- **Inverso**, se $u\in V_2$ e $v\in V_1$

La **Capacità del taglio** $\color{#CC241D}u[V_1,V_2]$ è pari alla *somma delle capacità degli archi diretti* del taglio.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20240711095913.png]]
>$$V_1 =\{1,2,3\},\quad V_2 = \{4,5,6\}$$
>Archi diretti: $\{(1,4), (2,4), (2,5), (3,6)\}$
>Capacità $u[V_1,V_2]$: $6 + 5 + 12 + 4 = 27$

### Grafo Ausiliario
Dato un grafo $G=(V,E)$ ed un flusso ammissibile $\underline{x}$ su $G$, il **grafo ausiliario** $G(\underline{x})=(V',E')$ è così costruito:
- $V'=V$
- Per ogni arco $(i,j)\in E$, $E'$ contiene $(i,j)$ e $(j,i)$
- La capacità $u_{i,j}$ di ogni arco $(i,j)\notin E$ è uguale a $0$
- Ad ogni arco di $E'$ è associata una **capacità residua** $r_{i,j}=u_{i,j}-x_{i,j}+x_{j,i}$

![[Pasted image 20240711100509.png]]

Inoltre si ha la seguente equivalenza nel grafo.

![[Pasted image 20240711103103.png]]


> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240711101004.png]]
>
>Diciamo di avere il seguente flusso ammissibile:
>$\underline{x}^T=[x_{1,2}\ x_{1,4}\ x_{2,3}\ x_{2,4}\ x_{2,5}\ x_{3,6}\ x_{4,3}\ x_{4,5}\ x_{5,3}\ x_{5,6}]=[3\ 1\ 3\ 0\ 0\ 3\ 0\ 1\ 0\ 1]$
>
>Allora abbiamo che:
>$r_{1,2}=10-3+0=7$
>$r_{2,1}=0-0+3=3$
>$r_{1,4}=6-1+0=5$
>$r_{4,1}=0-0+3=1$
>$r_{2,3}=15-3+0=12$
>$r_{3,2}=0-0+3=3$
>$r_{2,4}=5-0+0=5$
>$r_{4,2}=0-0+0=0$
>$r_{2,5}=12-0+0=12$
>$r_{5,2}=0-0+0=0$
>$r_{3,6}=4-3+0=1$
>$r_{6,3}=0-0+3=3$
>$r_{4,3}=8-0+0=8$
>$r_{3,4}=0-0+0=0$
>$r_{4,5}=10-1+0=9$
>$r_{5,4}=0-0+1=1$
>$r_{5,3}=6-0+0=6$
>$r_{3,5}=0-0+0=0$
>$r_{5,6}=15-1+0=14$
>$r_{6,5}=0-0+1=1$
>
>![[Pasted image 20240711102011.png]]

Se un arco ha capacità residua *maggiore di zero*, significa che *posso spedire ancora del flusso* attraverso quell'arco.
