---
aliases:
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Grafi
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

![[021 Algoritmo di Prim#^e8296c]]


## Albero dei cammini minimi con Prim
Seguire l'algoritmo di sopra.

Per indicare il valore delle variabili in corrispondenza della soluzione ottima individuata, si enumera $$x_{i,j}=c^{MST}_{i,j}$$
dove $c^{MST}_{i,j}$ è il costo sull'arco $(i,j)$ dell'MST. Se questo arco non è presente, allora il costo è 0.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240712103601.png]]
>
>| Nodi MST                                    | Archi MST                  | Costo arco aggiunto |
>| ------------------------------------------- | -------------------------- | :-----------------: |
>| $(1,2)$                                     | $1,2$                      |          1          |
>| $(1,2),(1,3)$                               | $1,2,3$                    |          2          |
>| $(1,2),(1,3),(3,4)$                         | $1,2,3,4$                  |          1          |
>| $(1,2),(1,3),(3,4),(4,5)$                   | $1,2,3,4,5$                |          1          |
>| $(1,2),(1,3),(3,4),(4,5),(5,7)$             | $1,2,3,4,5,7$              |          1          |
>| $(1,2),(1,3),(3,4),(4,5),(5,7),(2,6)$       | $1,2,3,4,5,7,6$            |          2          |
>| $(1,2),(1,3),(3,4),(4,5),(5,7),(2,6),(6,8)$ | $1,2,3,4,5,7,6,8=V$ (FINE) |          5          |
>
>$$\text{Costo}=1+2+1+1+1+2+5=13$$
>Valori delle variabili:
>$x_{1,2}=1$
>$x_{1,3}=2$
>$x_{3,4}=1$
>$x_{4,5}=1$
>$x_{5,7}=1$
>$x_{2,6}=2$
>$x_{6,8}=5$
