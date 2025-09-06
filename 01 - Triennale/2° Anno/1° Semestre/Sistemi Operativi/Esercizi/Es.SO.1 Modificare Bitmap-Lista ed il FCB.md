---
tags:
  - corsi/informatica/sistemi_operativi
  - esercizi
---
## MODIFICARE IL FCB
Il [[File Control Block]] contiene oggetti specificati dagli esercizi.
Principalmente contiene: 
- il blocco di partenza del file ed il numero di blocchi (se è ad [[012 Allocazione contigua di file|allocazione contigua]] o [[013 Allocazione linkata di file|linkata]]);
- alcuni blocchi indice, puntatori a blocchi che contengono puntatori ai dati ed il numero di blocchi (se è ad [[014 Allocazione indicizzata di file|allocazione indicizzata]]).

Si modificano i campi direttamente.

![[Pasted image 20221010173305.png|400]]

## MODIFICARE LA BITMAP
Nella [[015 Gestione dello spazio libero#VETTORE DI BIT BITMAP|bitmap]] si riscrive semplicemente 1 se un blocco è considerato vuoto, 0 altrimenti.

![[Pasted image 20221010115716.png|400]]

## MODIFICARE LA LISTA DEI BLOCCHI LIBERI
Nella [[015 Gestione dello spazio libero#LISTA CON BLOCCHI LIBERI|lista dei blocchi liberi]] si può:
1. **Aggiungere** un blocco libero alla lista: ![[Pasted image 20221010184119.png]]
	- Se necessario (quando il blocco non è il primo), portare in MP il blocco;
	- Si scrive l'head della lista dei blocchi liberi all'interno del blocco da liberare (negli ultimi 4 byte in modo da farlo diventare un puntatore);
	- Si cambia l'head della lista;
	- Si cambia il FCB.

2. **Rimuovere** un blocco libero dalla lista: ![[Pasted image 20221010190155.png]]
	- Si porta il nodo head della lista dei blocchi liberi alla MP;
	- Si cambia l'head con il prossimo nodo ottenuto dal puntatore;
	- Si scrivono i dati necessari nel blocco;
	- Cambiare il FCB;
	- Si riporta al DISCO il blocco.



%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%
