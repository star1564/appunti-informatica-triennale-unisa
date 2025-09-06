---
aliases:
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Grafi
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

> [!tip]- Minimum Spanning Tree (Algoritmi)
>![[020 Minimo Sottografo Connesso Ricoprente|Minimum Spanning Tree]]

## Subtour Elimination

$$\min\displaystyle\sum_{(i,j)\in E} c_{i,j}\ x_{i,j}$$
$$\displaystyle\sum_{(i,j)\in E} x_{i,j}=n-1$$
$$\displaystyle\sum_{(i,j)\in E,\ i,j\in S} x_{i,j}\leq |S| -1,\quad \forall S\subset V,\quad |S|\geq 3$$
$$x_{i,j}=\begin{cases} 1\quad \text{se l'arco }(i,j)\text{ è selezionato} \\ 0\quad \text{altrimenti} \end{cases}$$

## Cut Formulation

$$\min\displaystyle\sum_{(i,j)\in E} c_{i,j}\ x_{i,j}$$
$$\displaystyle\sum_{(i,j)\in E} x_{i,j}=n-1$$
$$\displaystyle\sum_{(i,j)\in E,\ i\in S,\ j\in V-S} x_{i,j}\geq 1,\quad \forall S\subset V,\quad |S|\geq 1$$
$$x_{i,j}=\begin{cases} 1\quad \text{se l'arco }(i,j)\text{ è selezionato} \\ 0\quad \text{altrimenti} \end{cases}$$
