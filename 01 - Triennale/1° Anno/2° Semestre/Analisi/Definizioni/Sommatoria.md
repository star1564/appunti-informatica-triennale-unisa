---
tags:
  - corsi/matematica/analisi
---
La **Sommatoria** permette di scrivere in modo compatto la somma di un numero finito o infinito di termini.

Il simbolo è $\color{#CC241D}\sum$ (Sigma Maiuscolo) ma scritto da solo non ha significato. Infatti la sommatoria è composta dai seguenti elementi:

![[Pasted image 20230303143106.png]]

> [!tip]+ Sommatoria come Codice
>In programmazione, questa la si può ottenere attraverso un ciclo `for`:
>```C
>int sum = 0;
>for(int k = n; k <= m; k++)
>	sum += f[k];
>return sum;
>```

> [!example]- <font color="orange">Esempio</font>
>$\displaystyle\sum_{k=0}^{5} k = 0+1+2+3+4+5 = 15$
>
>---
>$\displaystyle\sum_{k=3}^{8} (k-2) = (3-2) + (4-2) + (5-2) + (6-2) + (7-2) + (8-2) = 21$
>
>---
>Data una sequenza $a = a[0]a[1]\cdots a[n-1]$ di numeri, vogliamo calcolare $V(i,j)$, ovvero la somma degli elementi della sequenza $a$ dall'$i$-esimo allo $j$-esimo, consecutivamente. Dove $0\leq i\leq j \leq n-1$.
>$V(i,j)=\displaystyle\sum_{k=i}^{j} a[k] = a[i] + a[i+1] + \cdots + a[j]$

%%
[[000 Indice Analisi]]
%%
