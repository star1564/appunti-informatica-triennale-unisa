---
tags:
  - corsi/informatica/sistemi_operativi
---
Il metodo della **Tabella di Allocazione dei File (FAT)** è usato nei sistemi Windows e consiste in una tabella memorizzata nel disco.

È come un array che contiene tanti elementi quanti sono i blocchi.
In `FAT[i]` è contenuto il numero del blocco successivo al blocco `i`.

La directory entry contiene il numero del primo blocco che sarà usato come indice iniziale della FAT.
L'entry della FAT che corrisponde a blocchi inutilizzati contiene $0$.

La tabella FAT *si trova nella Memoria Principale*, quindi se si eseguono solo operazioni di ricerca, non si hanno [[Accesso al Disco|accessi al disco]].

![[Pasted image 20221006163025.png|500]]


%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%