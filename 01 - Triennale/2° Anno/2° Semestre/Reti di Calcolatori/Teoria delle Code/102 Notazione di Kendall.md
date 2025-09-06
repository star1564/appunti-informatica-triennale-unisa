---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Teoria delle Code
cssclasses:
  - 
---
Nel 1953 David George Kendall introdusse una notazione capace di poter indicare in modo unitario tutti gli elementi caratteristici di un [[101 Teoria delle Code|sistema a coda]]: $$\color{#61AFEF}A/B/s/c/p/Z$$
- $\color{#61AFEF}A$ rappresenta lo [[101 Teoria delle Code#^2bb0f3|schema di arrivo]];
- $\color{#61AFEF}B$ rappresenta lo [[101 Teoria delle Code#^614518|schema di servizio]];
- $\color{#61AFEF}s$ rappresenta il [[101 Teoria delle Code#^289f68|numero di serventi]] (intero non negativo);
- $\color{#61AFEF}c$ rappresenta la [[101 Teoria delle Code#^e5d338|capacità del sistema]] (intero non negativo), se non indicata si assume infinita;
- $\color{#61AFEF}p$ rappresenta la [[101 Teoria delle Code#^efe251|dimensione della popolazione]] (intero non negativo), se non indicata si assume infinita;
- $\color{#61AFEF}Z$ rappresenta la [[101 Teoria delle Code#^df9c48|disciplina della coda]], se non indicata si assume FIFO.

Le distribuzioni di probabilità dello schema di arrivo e di servizio ($A, B$) *più frequentemente assunte* sono:
- Indicata con $\color{#61AFEF}M$, la *distribuzione esponenziale* o (*Markoviana*);
- Indicata con $\color{#61AFEF}D$, la *distribuzione costante (Degenere)* o (tempi deterministici);
- Indicata con $\color{#61AFEF}E_k$, la *distribuzione di Erlang di ordine $k$*;
- Indicata con $G$ una distribuzione generica ($GI$ per [[021 Eventi Indipendenti|eventi indipendenti]]).

Notazioni standard usate:
- $\color{#61AFEF}\lambda$ indica la frequenza media degli arrivi dei clienti nel sistema, ovvero il *numero medio di utenti arrivati* nell'unità di tempo. Dati i tempi di inter-arrivo, si ha come media dei tempi di inter-arrivo $E(t_i^a)$, pertanto $$\lambda=\frac{1}{E(t_i^a)}$$
 ^2bdac8
- $\color{#61AFEF}\mu$ indica la velocità di servizio, ovvero il *numero medio di utenti con le loro richieste soddisfatte* nell'unità di tempo $$\mu=\frac{1}{E(t_i^s)}$$
 ^e02df2
- $\color{#61AFEF}\rho$ indica il *fattore di utilizzazione dei serventi*, ovvero il rapporto tra la frequenza media degli arrivi e la velocità del servizio moltiplicata per il numero dei serventi: $$\rho=\frac{\lambda}{s\cdot\mu}$$
- $\color{#61AFEF}n(t)$ è lo stato di un sistema a coda al tempo $t$ e che il *numero di clienti presenti nel sistema*, quindi nella fila d'attesa;
 ^e49ae5
- $\color{#61AFEF}n^q(t)$ è la lunghezza della coda al tempo $t$: $$n^q(t)=\begin{cases} 0, \quad\quad\quad\quad se\ n(t)\leq s \\ n(t)-s, \quad se\ n(t)> s \end{cases}$$ ^f68ae8




___
[[000 Indice RC|↖ Ritorna all'indice ↖]] , [[101 Teoria delle Code|← Nota Precedente]] | [[103 Misure di Prestazione|Nota Successiva →]]

Altri collegamenti: 