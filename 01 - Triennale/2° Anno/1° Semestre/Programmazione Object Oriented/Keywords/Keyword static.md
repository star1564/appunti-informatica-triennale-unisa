---
aliases:
  - static
tags:
  - corsi/informatica/programmazione_object_oriented
---
Sappiamo che da una [[Classi|Classe]] possiamo ottenere molteplici istanze e per ciascuna istanza si hanno variabili dai nomi identici ma dai valori distinti. 

Se poi vogliamo che una variabile sia la *medesima per tutte le istanze* di una classe, la dobbiamo invece definire come **`static`**.

```Java
// Questo indica che la variabile pi non può essere modificata, non può essere vista dall'esterno
// ed è la stessa per ogni istanza della classe
private static final double pi = 3.141592;
```

- [[Keyword private|private ↩]];
- [[Keyword final|final ↩]].

Per i [[Metodi]] avviene sostanzialmente la medesima cosa: possiamo pensare che dei metodi definiti in una classe ne esista normalmente (cioè se non si specifica `static`) una "copia" per ogni istanza della classe, mentre dei metodi statici ne esista *una sola copia associata alla classe stessa*. Per scendere più in dettaglio:

- I **metodi non statici** sono associati ad ogni singola istanza di una classe e perciò il loro contesto di esecuzione (quindi l'insieme delle variabili cui possono accedere) è relativo all'istanza stessa;
	- possono accedere e modificare le variabili dell'istanza e modificarne lo stato.
- I **metodi statici** non sono associati ad una istanza ma solo ad una classe. Quindi non potranno interagire con le variabili di istanza, ma solamente con quelle statiche.

Questa distinzione tra metodi statici e metodi di istanza si riflette anche in una diversa sintassi che si deve utilizzare per eseguire i 2 tipi di metodi:

- *Statico*: `NomeClasse.nomeMetodo(...)`;
- *Non Statico (Istanza)*: `nomeOggetto.nomeMetodo(...)`.

%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%