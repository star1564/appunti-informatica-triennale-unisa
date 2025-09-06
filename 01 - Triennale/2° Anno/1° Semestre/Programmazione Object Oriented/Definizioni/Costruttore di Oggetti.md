---
aliases:
  - Costruttore
cssclasses:
  - 
tags:
  - corsi/informatica/programmazione_object_oriented
---
Un **Costruttore** è molto simile a un metodo, con due differenze rilevanti:
- Il nome di un costruttore è *sempre* uguale al nome della classe;
- I costruttori non definiscono un tipo per il valore di ritorno (nemmeno void).

Se vogliamo dare la possibilità di poter costruire un oggetto con dati ottenuti dai parametri specificati e di oggetti senza parametri, specifichiamo due costruttori:
- `public nomeCostruttore(parametri)`;
- `public nomeCostruttore()`.

Il fatto che ci siano due costruttori con lo stesso nome non infastidisce il compilatore, che li considererà due costruttori diversi.

Il primo richiede parametri, il secondo no.


%%[[000 Indice POO|↖ Ritorna all'indice ↖]]%%