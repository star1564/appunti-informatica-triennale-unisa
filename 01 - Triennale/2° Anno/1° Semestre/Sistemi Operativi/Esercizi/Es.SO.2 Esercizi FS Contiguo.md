---
tags:
  - corsi/informatica/sistemi_operativi
  - esercizi
---
Alcuni esempi di esercizi su un [[012 Allocazione contigua di file|file system ad allocazione contigua]].
In questo tipo di allocazione si può accedere direttamente ad un blocco (es $b+3$) e portarlo in MP.

### AGGIUNGERE UN BLOCCO ALL'INIZIO
![[Pasted image 20221010193429.png]]
- Si shiftano i blocchi verso il basso (dove si trova lo spazio libero) a partire dall'ultimo elemento fino a quando si sposta anche il primo blocco;
- Si scrive in posizione $b$ il blocco da aggiungere;
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE IL FCB|cambia il FCB]];
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE LA BITMAP|cambia la bitmap]].

Ci sono stati $13$ [[Accesso al Disco|Accessi al Disco]].

### RIMUOVERE IL PRIMO BLOCCO
- Semplice e veloce: [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE IL FCB|cambia il blocco iniziale del FCB]] e [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE LA BITMAP|cambia la bitmap]]. ![[Pasted image 20221010194325.png]]
- Lento: Shiftare i blocchi verso l'alto, sovrascrivendo il primo blocco, modificare il FCB e la bitmap. ![[Pasted image 20221010194757.png]] Ci sono stati $10$ [[Accesso al Disco|Accessi al Disco]].

%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%