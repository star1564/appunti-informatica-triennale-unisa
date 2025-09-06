---
aliases:
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Grafi
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

![[022 Algoritmo di Kruskal#^3198b4]]

## Albero dei cammini minimi con Kruskal

Seguire l'algoritmo di sopra. Si ferma quando
$$|E|=|V|-1$$

Per indicare il valore delle variabili in corrispondenza della soluzione ottima individuata, si enumera $$x_{i,j}=c^{MST}_{i,j}$$
dove $c^{MST}_{i,j}$ è il costo sull'arco $(i,j)$ dell'MST. Se questo arco non è presente, allora il costo è 0.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240712101723.png]]
>
>|  Arco   | Costo | Accettato nell'MST | Numero Archi MST |   Numero Nodi MST    |
>| :-----: | :---: | :----------------: | :--------------: | :------------------: |
>| $(1,3)$ |  $1$  |         ✔️         |        1         |          2           |
>| $(1,4)$ |  $1$  |         ✔️         |        2         |          3           |
>| $(3,4)$ |  $1$  |         ❌          |                  |                      |
>| $(4,5)$ |  $1$  |         ✔️         |        3         |          4           |
>| $(1,2)$ |  $2$  |         ✔️         |        4         |          5           |
>| $(5,2)$ |  $2$  |         ❌          |                  |                      |
>| $(3,7)$ |  $2$  |         ✔️         |        5         |          6           |
>| $(4,7)$ |  $2$  |         ❌          |                  |                      |
>| $(2,4)$ |  $3$  |         ❌          |                  |                      |
>| $(5,6)$ |  $3$  |         ✔️          |        6         |          7           |
>| $(5,6)$ |  $4$  |         ❌          |                  |                      |
>| $(5,7)$ |  $4$  |         ❌          |                  |                      |
>| $(6,8)$ |  $5$  |         ✔️         |        7         | 8 (FERMA QUI, 8-1=7) |
>| $(5,8)$ |  $6$  |                    |                  |                      |
>| $(7,8)$ |  $6$  |                    |                  |                      |
>
>$$\text{Costo} = 1+1+1+2+2+3+5 = 15$$
>
>![[Pasted image 20240712102109.png]]
️
>Valori delle variabili:
>$x_{1,3}=1$
>$x_{1,4}=1$
>$x_{3,4}=0$
>$x_{4,5}=1$
>$x_{1,2}=2$
>$x_{5,2}=0$
>$x_{3,7}=2$
>$x_{4,7}=0$
>$x_{2,4}=0$
>$x_{5,6}=3$
>$x_{5,6}=0$
>$x_{5,7}=0$
>$x_{6,8}=5$
>$x_{5,8}=0$
>$x_{7,8}=0$


