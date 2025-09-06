---
tags:
  - corsi/informatica/basi_di_dati
---
- **Attributi Composti**:
Possono essere *divisi in parti più piccole*, che rappresentano più attributi di base con significati indipendenti.
Per esempio, un attributo "Indirizzo" può essere diviso (se richiesto) in "Via", "Città", "Stato" e "CAP".
![[Pasted image 20221008110924.png]]
Gli attributi composti possono formare una *gerarchia*: per esempio "Indirizzo_Via" può essere diviso in tre attributi: "Numero", "Nome" e "Interno".
 ^b63ded
- **Attributi Semplici**: 
Sono degli attributi che *non sono divisibili*.
Per esempio l'attributo "Indirizzo_Stato" non è divisibile.
 ^9c4be0
- **Attributi Single-Value**: 
*Hanno un solo valore* per ciascuna entità.
Per esempio l'attributo "Età" ha un solo valore.

- **Attributi Multi-Valued**: 
Possono avere *un insieme di valori* per la stessa entità.
Per esempio l'attributo "Colori" di una macchina è multi-valued:
	- Se una macchina ha un solo colore, allora ha un valore singolo;
	- Se una macchina è bicolore, allora ha due valori.
Può essere determinato un [[007 Estremo Inferiore e Superiore di un insieme|limite inferiore e superiore]] al numero di valori per un'entità.
Si rappresenta graficamente attraverso un *Ellisse con due bordi*.
 ^abce31
- **Attributi Derivati**:
In certi casi i valori di due o più attributi sono *correlati*. 
Per esempio, l'età e la data di nascita di una persona: l'età di una persona *può essere determinata* a partire dalla data corrente e dal valore della data di nascita.
"Età" è quindi un Attributo Derivato.
 ^cab5d8
- **Attributo Memorizzato**:
Quando un attributo non può essere [[#^cab5d8|derivato]], lo si memorizza.


%%[[000 Indice BD|↖ Ritorna all'indice ↖]]%%