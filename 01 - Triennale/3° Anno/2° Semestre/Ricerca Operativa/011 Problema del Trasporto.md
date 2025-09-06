---
aliases:
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Problemi su Grafi
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

## Definizione
>Il **Problema del Trasporto** è un tipo particolare di Problema di programmazione lineare, dove delle merci sono trasportate da un insieme di *fornitori* ad un insieme di *destinazioni* e sono soggetti alla *offerta e alla domanda* dei fornitori e delle destinazioni rispettivamente, in modo che *il costo di trasporto sia minimizzato*.

- La quantità totale delle merci trasportate da ciascun fornitore deve essere uguale alla offerta del fornitore stesso
- La quantità totale delle merci ricevute da ciascuna destinazione deve essere uguale a quella richiesta

Quando la *somma delle offerte è uguale alla somma delle domande*, si dice che è un problema di trasporto **bilanciato**.

Per risolvere il problema, lo si può rappresentare tramite due tabelle, una relativa alle variabili e l'altra relativa ai costi. Composte da:
- $F_i$: Fornitore
- $D_i$: Destinazione
- $c_{i,j}$: Costo per trasportare merci dal fornitore $i$ alla destinazione $j$
- $x_{i,j}$: Variabili
- $d_i$: Domanda della destinazione $D_i$
- $o_i$: Offerta del fornitore $F_i$

![[Pasted image 20240710163251.png|700]]
![[Pasted image 20240710163350.png|550]]

### Modello Matematico
$$\min \displaystyle\sum_{i=1}^{m}\displaystyle\sum_{j=1}^{n} c_{i,j}\ x_{i,j}$$
$$\boxed{1 \text{ - offerta}}\quad \displaystyle\sum_{j=1}^{n} x_{i,j}=o_i, \quad \forall i=1,\dots, m$$
$$\boxed{2 \text{ - domanda}}\quad -\displaystyle\sum_{i=1}^{m} x_{i,j}=-d_j, \quad \forall j=1,\dots, n$$
$$\boxed{3}\quad x_{i,j}\geq 0\quad \forall i=1,\dots,m\quad \forall j=1,\dots,n$$



> [!example]- <font color="orange">Esempio con Modello Matematico</font>
>![[Pasted image 20240710161558.png|600]]
>
>In questo caso si ha un problema bilanciato. $$4+2+3+3=\textcolor{#61AFEF}{12}=3+4+5$$
>
>Bisogna scrivere il modello matematico per questa istanza del problema del trasporto.
>
>$\min 2x_{1,1}+10x_{1,2}+2x_{1,3}+8x_{1,4}+$
>$\quad\quad+2x_{2,1}+3x_{2,2}+6x_{2,3}+10x_{2,4}+$
>$\quad\quad+8x_{3,1}+7x_{3,2}+3x_{3,3}+1x_{3,4}$
>
>Offerte:
>1. $x_{1,1}+x_{1,2}+x_{1,3}+x_{1,4}=3$
>2. $x_{2,1}+x_{2,2}+x_{2,3}+x_{2,4}=4$
>3. $x_{3,1}+x_{3,2}+x_{3,3}+x_{3,4}=5$
>
>Domande:
>1. $-x_{1,1}-x_{2,1}-x_{3,1}=-4$
>2. $-x_{1,2}-x_{2,2}-x_{3,2}=-2$
>3. $-x_{1,3}-x_{2,3}-x_{3,3}=-3$
>4. $-x_{1,4}-x_{2,4}-x_{3,4}=-3$

## Risoluzione
Esistono due modi per risolvere un problema del trasporto.

### Metodo Angolo Nord-Ovest
La dimensione della base è pari a $$n+m-1$$

1. Poni $x_{i,j}=0$ per ogni $i$ e per ogni $j$
2. $i=1,j=1$
3. 🔁 $x_{i,j}=\min\{o_i, d_j\}$
	- Se il minimo è $o_i$, allora $i=i+1, d_j=d_j-o_i$
	- Se il minimo è $d_i$, allora $j=j+1, o_j=o_j-d_i$


Le variabili di base della soluzione iniziale sono i quelle variabili diverse da zero dopo l'esecuzione dell'algoritmo di sopra.

Questa soluzione è **ammissibile**, ma bisogna vedere *se è ottima*. Se non lo è, allora bisogna cercare un'altra soluzione con la regola del ciclo.

Dobbiamo verificare i valori $z_{i,j}-c_{i,j}$ per ogni $x_{i,j}$ non in base.

Il calcolo di queste differenze si riduce al calcolo delle differenze dei valori delle variabili duali associate ai vincoli (*condizione di ottimalità*):
$$z_{i,j}-c_{i,j}=u_i-v_j-c_{i,j}$$
Dove:
- $u_i$ è la variabile duale associata all'$i$-esimo vincolo di origine, $u_i=0$ arbitrariamente $$u_i=c_{i,j}+v_j$$
- $v_j$ è la variabile duale associata all'$j$-esimo vincolo di destinazione $$v_j=u_i-c_{i,j}$$

Se tutte le soluzioni fuori base $z_{i,j}-c_{i,j}$ sono $\leq 0$, allora questa è una **soluzione ottima**, altrimenti non lo è. 
Se non lo è, la soluzione *più grande* $\geq 0$ sarà la *variabile entrante*, poi si procede con la regola del ciclo.

> [!warning] Attenzione
> Durante il calcolo della base con il metodo Nord-Ovest può capitare di trovarsi uno $0$ come minimo tra domanda e offerta.
> Si tratta di un problema del trasporto **degenere** e lo si risolve mettendo uno $0$ e proseguendo normalmente con i calcoli e si dovrebbe risolvere al primo passaggio, ovvero che la base è già ottima.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240710171714.png]]
>
>$$n+m-1=4+3-1=6$$
> 
 >![[Pasted image 20240710171650.png|700]]
 >![[Pasted image 20240710171907.png]]
>Le variabili di base della soluzione ammissibile iniziale sono:
>$$x_{1,1},x_{1,2}, x_{2,2}, x_{2,3}, x_{3,3}, x_{3,4}$$
>
>Ora dobbiamo verificare se questa soluzione è ottimale:
>
>$$\begin{array}{c|c}
x_{1,1} & u_1-v_1=c_{1,1}=10 \\
x_{1,2} & u_1-v_2=c_{1,2}=5 \\
x_{2,2} & u_2-v_2=c_{2,2}=2 \\
x_{2,3} & u_2-v_3=c_{2,3}=7 \\
x_{3,3} & u_3-v_3=c_{3,3}=4 \\
x_{3,4} & u_3-v_4=c_{3,4}=8 \\
\end{array}$$
>
>
>$$\begin{array}{c|cc}
x_{1,1} & u_1=0  & v_1=-10 \\
x_{1,2} & u_1=0  & v_2=-5  \\ 
x_{2,2} & u_2=-3 & v_2=-5  \\
x_{2,3} & u_2=-3 & v_3=-10  \\
x_{3,3} & u_3=-6 & v_3=-10  \\
x_{3,4} & u_3=-6 & v_4=-14  \\
\end{array}$$
>
>
>|     |  u  |  v  |
>| :-: | :-: | :-: |
>|  1  |  $0$  | $-10$ |
>|  2  | $-3$  | $-5$  |
>|  3  | $-6$  | $-10$ |
>|  4  |     | $-14$ |
>
>
>
>Ora verifichiamo le soluzioni non in base:
>$z_{1,3}-c_{1,3}=u_1-v_3-c_{1,3}=-(-10)-6=\color{#CC241D}4$
>$z_{1,4}-c_{1,4}=u_1-v_4-c_{1,4}=-(-14)-7=\color{#00C575}7$
>$z_{2,1}-c_{2,1}=u_2-v_1-c_{2,1}=-3-(-10)-8=-1$
>$z_{2,4}-c_{2,4}=u_2-v_4-c_{2,4}=-3-(-14)-6=\color{#CC241D}5$
>$z_{3,1}-c_{3,1}=u_3-v_1-c_{3,1}=-6-(-10)-9=-5$
>$z_{3,2}-c_{3,2}=u_3-v_2-c_{3,2}=-6-(-5)-3=-4$
>
>Abbiamo delle soluzioni non in base $\color{#CC241D}\geq 0$, quindi la soluzione *non è ottima*.
>Dobbiamo scegliere una variabile non in base da introdurre in base. Facciamo entrare in base la variabile con coefficiente di costo massimo ossia $\color{#00C575}x_{1,4}$.
### Regola del Ciclo
Questa si applica dopo aver provato ad utilizzare il metodo Nord-Ovest. 

1. Si seleziona la variabile uscente e si scrive $y$
2. Consideriamo le variabili che formano un ciclo con la variabile entrante, si ci può muovere solamente su nodi in base
3. Incrementiamo la variabile entrante da 0 ad un nuovo valore $\Delta>0$
4. Le variabili di base coinvolte nel ciclo verranno incrementate di $\Delta$, se hanno segno positivo, mentre verranno decrementate di $\Delta$, se hanno segno negativo
5. La variabile uscente sarà quella che si azzera per prima
6. Si ripete il calcolo delle variabili fuori base $(z-c)$ e si verifica se tutte le soluzioni sono $\leq0$. Se lo sono, allora la base è ottima ed il problema è risolto

> [!example]- <font color="orange">Esempio</font>
>- Variabile uscente: $x_{1,4}$
>- $\Delta=10$, il minimo tra tutte le variabili coinvolte nel ciclo con segno $-$
>
>![[Pasted image 20240710183723.png]]


### Funzione obbiettivo della soluzione ottima

Per vedere il valore delle *variabili decisionali*, si prendono i valori della base ottima.

Invece, per vedere la *funzione obbiettivo* si prendono le variabili della base ottima, con i valori del problema originale.

> [!example]- <font color="orange">Esempio</font>
>Base ottima:
>![[Pasted image 20240711091843.png]]
>
>Problema originale:
>![[Pasted image 20240711092332.png]]
>
>**Variabili decisionali**:
>$$x_{1,1}=15, x_{1,4}=10, x_{2,2}=20, x_{2,3}=5, x_{3,3}=25, x_{3,4}=25$$
>
>**Funzione obbiettivo**:
>$$z^* =10x_{1,1} + 7x_{1,4} + 2x_{2,2} + 7x_{2,3} + 4x_{3,3} + 8x_{3,4}$$

