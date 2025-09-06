---
tags:
  - corsi/informatica/elementi_teoria_computazione
---
Questo paradosso si manifesta nel momento in cui si considera *l'insieme di tutti* gli insiemi che non sono elementi di se stessi.

> [!tip] Insiemi che non sono elementi di se stessi
> Sono quelli che non contengono se stessi come membri. 
> Ad esempio, l'insieme delle città in Italia non è una città, quindi non può essere un elemento di se stesso.

Consideriamo tre insiemi:
- $A$, l'insieme di tutti gli insiemi finiti
- $B$, l'insieme di tutti gli insiemi infiniti
- $C$, l'insieme di tutti gli insiemi che non sono elementi di se stessi

Allora:
- $A\not\in A$
	- Il numero di insiemi finiti è infinitamente grande, quindi $A$ è un insieme *infinito*. Pertanto $A$ non può essere un elemento di se stesso, poiché include solo insiemi *finiti* e $A$ è infinito.
- $B\in B$
	- Il numero di insiemi infiniti è infinitamente grande, quindi $B$ è un insieme *infinito*. Pertanto $B$ è un elemento di se stesso, poiché include tutti gli insiemi *infiniti* e $B$ è un insieme infinito.
- $A\in C$ 
	- Abbiamo visto che $A$ è un insieme infinito e *non può essere elemento di se stesso*. Pertanto, $A$ appartiene a $C$.
- $B\not\in C$
	- Abbiamo visto che $B$ è un insieme infinito e *può essere elemento di se stesso*. Pertanto, $B$ non appartiene a $C$.
- $C\in C\ ?$
	- Se $C$ fosse un elemento di se stesso, allora, per definizione, non dovrebbe appartenere a se stesso. Questo crea una contraddizione. Stessa cosa se proviamo a vedere se $C$ non è un elemento di se stesso. Questo ragionamento porta al **Paradosso di Russell**, che mostra che la definizione di $C$ porta a una contraddizione intrinseca.

%%
[[000 Indice ETC|↖ Ritorna all'indice ↖]]
%%