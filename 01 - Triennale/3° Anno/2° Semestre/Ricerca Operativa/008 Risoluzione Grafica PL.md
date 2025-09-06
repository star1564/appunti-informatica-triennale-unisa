---
aliases: 
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Programmazione Lineare
cssclasses:
  - 
---

> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

> [!quote] Traccia Esempio
>$$
>\begin{gather*}
>\color{#CC241D}\min\quad -x_1 + 2x_2 \\
>\begin{aligned}
>\textup{s.t.}\quad 
>\color{#61AFEF}2x_1 - x_2  &\geq  0 \\
>\color{#61AFEF}x_1 - x_2  &\leq  6 \\
>\color{#61AFEF}-x_1 + 3x_2  &\geq  -3 \\
>\color{#00C575}x_1,x_2 &\geq0 \\
>\end{aligned}
>\end{gather*}
>$$
>
>Da notare che la soluzione si troverà per forza nel primo quadrante in quanto si ha che $\color{#00C575}x_1\ge0, x_2\ge0$.




## Risoluzione Grafica, punto di ottimo e soluzione ottima


1. Numerare i vincoli


$$
\begin{aligned}
&\boxed{1}\quad\color{#61AFEF}2x_1-x_2 \ge0 \\
&\boxed{2}\quad\color{#61AFEF}x_1-x_2\le6 \\
&\boxed{3}\quad\color{#61AFEF}-x_1+3x_2\ge-3 \\
\end{aligned}
$$

2. Impostare i vincoli con $=$ invece di $<,>,\le,\ge$

$$
\begin{aligned}
&\boxed{1}\quad2x_1-x_2 =0 \\
&\boxed{2}\quad x_1-x_2=6 \\
&\boxed{3}\quad-x_1+3x_2=-3 \\
\end{aligned}
$$


3. Risolvere ogni funzione e disegnare le rette

![[Pasted image 20240708125718.png|1100]]


4. Vedere qual è la "direzione" della soluzione. L'area che interseca questi versi è la **[[Regione Ammissibile]]**. 
   
   Per fare questo, si devono eseguire questi passi su ogni funzione: 
	- Si sceglie un punto al di fuori della retta, su un lato. 
	- Se questo punto soddisfa il vincolo originale (quello con $<,>,\le,\ge$) allora il lato scelto va bene e si inseriscono due frecce sulla retta nella direzione della soluzione
	- Da notare che un lato può avere punti che soddisfano e che non soddisfano il vincolo originale


$$
\begin{aligned}
&\boxed{1}\quad(1,1)=2-1=1\text{ soddisfa} \geq0 \\
&\boxed{2}\quad (0,0)=0-0=0\text{ soddisfa} \leq6 \\
&\boxed{3}\quad(0,0)=0+0=0\text{ soddisfa} \geq-3 \\
\end{aligned}
$$

![[Pasted image 20240708125616.png|1100]]


5. Individuare i vertici della regione ammissibile. $A,B,O$ sono i tre punti estremi della regione ammissibile, ci interessano perché per il teorema fondamentale della programmazione lineare, questi vertici potrebbero essere il **[[Punto di ottimo]]**.
   
   Se non si sanno le coordinate di un punto, è possibile calcolarle mettendo le due rette che creano il punto in un sistema e risolverlo.


![[Pasted image 20240708141021.png|1100]]

$$O=(0,0)$$
$$A=(3,0)$$
$$B=\begin{cases} x_1-x_2=6 \\ -x_1+3x_2=-3 \end{cases}=\begin{cases} x_1=x_2+6 \\ x_2=\frac{3}{2} \end{cases}=\bigg(\frac{15}{2}, \frac{3}{2}\bigg)$$


6. Le soluzioni di base di un problema sono i vertici del poliedro. Essendo questo un problema di ottimo infinito, l'ottimo si trova in uno dei vertici del poliedro. Questo lo si può trovare con il metodo grafico attraverso il [[Gradiente]].  ^61efc1
   - Si prende la funzione obbiettivo e si ricava il suo vettore, prendendo le coordinate "direttamente" dalla funzione. Si fa partire il vertice dall'origine. Questo vettore è chiamato $\color{#CC241D}\underline{c}^T$ (nel grafico è $G$).
   - Si traccia una retta parallela alla direzione del gradiente. ^fe9e60
   - Si sposta questa retta in modo parallelo. Se la funzione è:
	   - Di *minimo*, la retta si ripeteranno nella direzione opposta al vettore del gradiente
	   - Di *massimo*, le rette si ripeteranno nella direzione del vettore del gradiente
   - L'ultimo punto della regione ammissibile che viene raggiunto dalla retta è il punto di ottimo.

$$\min -x_1+2x_2, \quad \underline{c}^T=[-1,2]$$

![[Pasted image 20240708153351.png|1100]]

$$\text{Il punto di ottimo è }B=\bigg(\frac{15}{2}, \frac{3}{2}\bigg)$$

7. La **soluzione ottima** viene calcolata sostituendo alla funzione obbiettivo le coordinate del punto di ottimo.

$$\color{#CC241D}z^*=-x_1+2x_2=-\frac{15}{2}+2\cdot \frac{3}{2}=-\frac{9}{2}$$ ^9cc3b0

8. Si può verificare che un punto è quello di ottimo comparando il risultato con tutti gli altri punti.

$$B=-4.5, A=-3, O=0$$
$$B \text{ offre il risultato minore}$$

## Basi
1. Il numero delle basi è pari al numero di vincoli che si trovano nel problema.

$$\text{3 vincoli = 3 basi}$$

2. Trasformare il problema nella [[Forma standard di un problema di PL|forma standard]], si utilizzano le <font color="#FF6611">variabili di surplus</font>, ossia variabili aggiunte per trasformare in uguaglianza. Il numero di queste variabili serve per far raggiungere ad ogni vincolo un numero di variabili pari a quello delle basi:
	- In caso di $\geq$ il segno è $-$
	- In caso di $\leq$ il segno è $+$

$$\min -x_1+2x_2$$
$$
\begin{aligned}
&\boxed{1}\quad2x_1-x_2 \textcolor{#FF6611}{-x_3} =0 \\
&\boxed{2}\quad x_1-x_2 \textcolor{#FF6611}{+x_4} =6 \\
&\boxed{3}\quad -x_1+3x_2 \textcolor{#FF6611}{-x_5}=-3 \\
\end{aligned}
$$
$$x_1,x_2,\textcolor{#FF6611}{x_3,x_4,x_5}\geq 0$$

> [!info] Se era max
> $\max -x_1+2x_2 \to -\min x_1-2x_2$

3. Tenere conto a quale vincolo è associata ciascuna variabile di surplus e porre ogni variabile $=0$. Da notare che $x_1$ è l'asse delle ordinate e che $x_2$ è quello delle ascisse.

![[Pasted image 20240708154306.png|1100]]



5. Vedere per ogni punto $P$, i vincoli la cui intersezione restituisce il punto $P$. Questi vincoli *sono fuori base* per quel punto, quindi basta vedere il complemento. Una base deve avere esattamente un certo numero di punti.

![[Pasted image 20240708155105.png|1100]]

$$A\to x_2=0\cap x_5=0 \to \{x_1,x_3,x_4\}, \quad Base_A = \{1,3,4\}$$
$$B\to x_4=0\cap x_5=0 \to \{x_1,x_2,x_3\}, \quad Base_B = \{1,2,3\}$$

6. Se all'origine sono associate più basi, questa si chiama **soluzione di box degenere**.

$$O^1\to x_2=0\cap x_3=0 \to \{x_1,x_4,x_5\}, \quad Base_O^1 = \{1,4,5\}$$
$$O^2\to x_1=0\cap x_3=0 \to \{x_2,x_4,x_5\}, \quad Base_O^2 = \{2,4,5\}$$
$$O^3\to x_1=0\cap x_2=0 \to \{x_3,x_4,x_5\}, \quad Base_O^3 = \{3,4,5\}$$

## Valore delle variabili nella base della soluzione ottima
La base ottima è quella inerente al punto ottimo. Quelle fuori base hanno valore 0 per definizione. Mentre quelle in base le si devono ricavare andando a sostituire le coordinate del punto all'interno dei vincoli.

Il punto di ottimo è $B=(\frac{15}{2}, \frac{3}{2})$, quindi la base ottima è $Base_B=\{1,2,3\}$. Pertanto, $x_4$ e $x_5$ sono fuori base, quindi hanno valore 0.

Dato che $x_1=\frac{15}{2}, x_2= \frac{3}{2}$, dobbiamo calcolare solo $x_3$. Questa variabile di surplus è inerente al vincolo $\boxed{1}$, quindi andiamo a sostituire i valori al suo interno per ottenere $x_3$. $$2x_1-x_2-x_3=0 \to x_3=2x_1-x_2\to \color{#CC241D}x_3=\frac{30}{2}-\frac{3}{2}=\frac{27}{2}$$

## Punti estremi (sistema omogeneo)

Si deve risolvere un sistema, impostato nel seguente modo:
- Sostituire $x_i$ con $d_i$
- Azzerare i termini noti
- Mettere la condizione di normalizzazione $d_1+d_2=1$, quindi che $d_1=1-d_2$
- Risolvere il sistema
- Sostituire alla condizione di normalizzazione la $d_2$ con i casi agli estremi ottenuti dal risultato
	- $\underline{d_1}$ sarà creato con l'estremo sinistro
	- $\underline{d_2}$ sarà creato con l'estremo destro

$$\begin{cases} 2d_1-d_2\geq 0 \\ d_1-d_2\leq 0 \\ -d_1+3d_2\geq 0 \\ d_1,d_2\geq 0 \\ d_1+d_2=1 \end{cases}\to\begin{cases} 2d_1-d_2\geq 0 \\ d_1-d_2\leq 0 \\ -d_1+3d_2\geq 0 \\ d_1,d_2\geq 0 \\ \textcolor{#FF6611}{d_1=1-d_2} \end{cases}\to\begin{cases} \textcolor{#FF6611}{2-2d_2-d_2\geq 0} \\ \textcolor{#FF6611}{1-d_2-d_2\leq 0} \\ \textcolor{#FF6611}{-1+d_2+3d_2\geq 0} \\ d_1,d_2\geq 0 \\ d_1+d_2=1 \end{cases}\to\begin{cases} \textcolor{#FF6611}{-3d_2\geq -2} \\ \textcolor{#FF6611}{-2d_2\leq -1} \\ \textcolor{#FF6611}{4d_2\geq 1} \\ d_1,d_2\geq 0 \\ d_1+d_2=1 \end{cases}\to\begin{cases} \textcolor{#FF6611}{d_2\leq \frac{2}{3}} \\ \textcolor{#FF6611}{d_2\geq \frac{1}{2}} \\ \textcolor{#FF6611}{d_2\geq \frac{1}{4}} \\ d_1,d_2\geq 0 \\ d_1+d_2=1 \end{cases}$$

![[Pasted image 20240708164954.png|600]]
$$\frac{1}{2}\le d_2\le \frac{2}{3}$$

Da notare che per via del fatto che $d_1+d_2=1$, si ha sempre che $0\le d_1,d_2\le1$.

1. Se scegliamo $\color{#e86162}d_2=\frac{1}{2}$ abbiamo che $\color{#00C575}d_1=1-\frac{1}{2}=\frac{1}{2}$, quindi $$\underline{d}^1=\begin{pmatrix} \textcolor{#00C575}{1/2} \\ \textcolor{#e86162}{1/2}  \end{pmatrix}$$

2. Se scegliamo $\color{#e86162}d_2=\frac{2}{3}$ abbiamo che $\color{#00C575}d_1=1-\frac{2}{3}=\frac{1}{3}$, quindi $$\underline{d}^2=\begin{pmatrix} \textcolor{#00C575}{1/3} \\ \textcolor{#e86162}{2/3}  \end{pmatrix}$$

## Teorema della Rappresentazione

### Con un problema di Minimo
1. Dalla [[Teorema della Rappresentazione|definizione]] possiamo ricavare che:
$$\min z=\sum_{i=1}^{k}\ \lambda_i(\underline{c}^T \underline{x}_i)\ +\ \sum_{j=1}^{t}\ \mu_j (\underline{c}^T\underline{d}_j)$$

Ricordando che:
- $\textcolor{#CC241D}{\underline{c}^T=[-1, 2]}$
- $\textcolor{#FF6611}{O(0,0)}$
- $\textcolor{#B35DB0}{A(3,0)}$
- $\textcolor{#00C575}{B\left( \frac{15}{2}, \frac{3}{2} \right)}$
- $\textcolor{#53ccc5}{\underline{d}^1=\begin{pmatrix} {1/2} \\ {1/2}  \end{pmatrix},\quad \underline{d}^2=\begin{pmatrix} {1/3} \\ {2/3}  \end{pmatrix}}$

Possiamo risolverlo andando a sostituire e ad espandere le sommatorie:
$$\min z=\lambda_1(\textcolor{#CC241D}{-1\ 2})\begin{pmatrix} \textcolor{#FF6611}0 \\ \textcolor{#FF6611}0  \end{pmatrix}+ \lambda_2(\textcolor{#CC241D}{-1\ 2})\begin{pmatrix} \textcolor{#B35DB0}3 \\ \textcolor{#B35DB0}0  \end{pmatrix}+ \lambda_3(\textcolor{#CC241D}{-1\ 2})\begin{pmatrix} \textcolor{#00C575}{15/2} \\ \textcolor{#00C575}{3/2}  \end{pmatrix}+\mu_1(\textcolor{#CC241D}{-1\ 2})\begin{pmatrix} \textcolor{#53ccc5}{1/2} \\ \textcolor{#53ccc5}{1/2}  \end{pmatrix}+\mu_2(\textcolor{#CC241D}{-1\ 2})\begin{pmatrix} \textcolor{#53ccc5}{1/3} \\ \textcolor{#53ccc5}{2/3}  \end{pmatrix}$$

Abbiamo quindi: 
$$\min z= 0\lambda_1 -3\lambda_2, -\frac{9}{2}\lambda_3+\frac{1}{2}\mu_1+1\mu_2$$
Con:
$$\lambda_1+\lambda_2+\lambda_3=1, \quad \lambda_1,\lambda_2,\lambda_3\geq0,\quad \mu_1,\mu_2\geq0$$

2. A questo punto possiamo avere due casi:
	- Se $\exists j:\underline{c}^T\underline{d}_j\color{#CC241D}<0$, allora abbiamo un *ottimo illimitato*
	- Se $\forall j:\underline{c}^T\underline{d}_j\color{#CC241D}\geq0$, allora abbiamo un *ottimo finito*

Il nostro problema ha $\frac{1}{2}\mu_1,1\mu_2$, che sono entrambi $\geq0$, quindi si ha un ottimo finito.

3. Bisogna capire i valori da assegnare alle incognite. 
	- Se si ha un ottimo illimitato, per un problema di *minimizzazione* impostiamo $\mu_j=-\infty, \forall j$
	- Se si ha un ottimo finito, impostiamo $\mu_j=0, \forall j$

Scelgo quindi che $\mu_1=\mu_2=0$.

$$\min z= 0\lambda_1 -3\lambda_2, \underbracket{-\frac{9}{2}}_\text{piccolo}\lambda_3$$
4. Dato che si vuole *minimizzare*, prendiamo il lambda con il coefficiente più *piccolo* e lo impostiamo ad 1, mentre gli altri a 0. In questo modo troviamo il **minimo possibile**.

Dato che $-\frac{9}{2}$ è il coefficiente più piccolo, impostiamo i lambda in questo modo:
$$\lambda_3=1,\quad\lambda_1=\lambda_2=0$$

Quindi, risolvendo abbiamo trovato il minimo possibile: 
$$z^*=-\frac{9}{2}$$

5. Quando risolve con il teorema della rappresentazione, si deve avere la stessa soluzione della risoluzione grafica

La soluzione è uguale alla [[#^9cc3b0|soluzione ottima trovata sopra]].

### Con un problema di Massimo
1. Dalla [[Teorema della Rappresentazione|definizione]] possiamo ricavare che:
$$\max z=\sum_{i=1}^{k}\ \lambda_i(\underline{c}^T \underline{x}_i)\ +\ \sum_{j=1}^{t}\ \mu_j (\underline{c}^T\underline{d}_j)$$

2. A questo punto possiamo avere due casi:
	- Se $\exists j:\underline{c}^T\underline{d}_j\color{#CC241D}>0$, allora abbiamo un *ottimo illimitato*
	- Se $\forall j:\underline{c}^T\underline{d}_j\color{#CC241D}\leq0$, allora abbiamo un *ottimo finito*

3. Bisogna capire i valori da assegnare alle incognite. 
	- Se si ha un ottimo illimitato, per un problema di *massimizzazione* impostiamo $\mu_j=+\infty, \forall j$
	- Se si ha un ottimo finito, per un problema di *massimizzazione* impostiamo $\mu_j=0, \forall j$

4. Dato che si vuole *massimizzare*, prendiamo il lambda con il coefficiente più *grande* e lo impostiamo ad 1, mentre gli altri a 0. In questo modo troviamo il **massimo possibile**.

5. Quando risolve con il teorema della rappresentazione, si deve avere la stessa soluzione della risoluzione grafica

## Funzione obbiettivo con ottimo illimitato
La traccia richiede di creare un nuova funzione obbiettivo per cui l'ottimo risulti illimitato. Questo lo si può fare utilizzando il gradiente come [[#^61efc1|sopra]], però le linee parallele alla direzione del gradiente *devono rientrare tutte all'interno della regione ammissibile*, che può essere infinita su un lato.

1. Per questo lavoro è più facile scegliere una funzione obbiettiva di massimo, in quanto la direzione delle linee segue quella del gradiente. 
2. Scegliere un punto adatto nel grafico per creare il gradiente.
3. Tracciare le frecce parallele per verificare se l'ottimo è illimitato.

$$\max 3x_1+x_2\text{ ha ottimo illimitato}$$

![[Pasted image 20240709113943.png|1100]]

Questo perché le rette parallele rientreranno sempre all'interno dell'area delle soluzioni ammissibili.

Vanno bene anche le seguenti funzioni:
- $\max x_1+2x_2$
- $\min -x_1-x_2$
- ...

## Vincolo per chiusa la regione ammissibile (politopo)

Nella traccia si vuole scegliere un nuovo vincolo per chiudere la regione ammissibile, ovvero farla diventare un politopo. Questo permette di avere un insieme delle soluzioni "finito".

$$\color{#FF6611}x_2\leq 5$$

![[Pasted image 20240709114720.png|1100]]

## Funzione obbiettivo con infiniti punti di ottimo
Si procede allo stesso modo della [[#Funzione obbiettivo con ottimo illimitato|funzione obbiettivo con ottimo illimitato]], ma in questo caso si possono prendere direttamente dei vincoli e trasformarli in funzione.

Le parallele che si creano dal gradiente devono coincidere in parte con un segmento del poligono.

$$\min -x_1+3x_2\quad \boxed{3}$$
Questo ha una coincidenza con la retta $AB$.

![[Pasted image 20240709120208.png|1100]]