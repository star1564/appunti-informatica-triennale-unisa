---
tags:
  - corsi/informatica/basi_di_dati
---
Il **Rapporto di Cardinalità** per una relazione specifica il numero (non negativo) *massimo e minimo* di istanze di relazione a cui può participate un'entità.

Per esempio nella seguente immagine ![[Pasted image 20221008145819.png]] si ha che:
- *MAX*, Ogni dipartimento può avere Numerosi impiegati, e ciascun impiegato lavora per Un solo dipartimento.
- *MIN*: Un dipartimento potrebbe non avere impiegati, mentre un impiegato deve essere sempre assegnato a un dipartimento.

Si ha in questo caso una [[ER7_Tipi Relazioni#^27ba3d|relazione N:1]], dove N impiegati lavorano per un dipartimento.

Si ogni lato si determina prendendo il Max dalla parte opposta (es.: Da parte di impiegato si vede che il max del lato opposto vicino a dipartimento è (0,N), quindi si ha N. Da parte di dipartimento si vede che si ha (1,1), quindi 1).

- Il valore 0 per la cardinalità minima indica una partecipazione **opzionale** del tipo di entità alla relazione.
- Il valore 1 indica una partecipazione **obbligatoria**.

> [!summary] Minimi e Massimi di una Relazione
>- (1,1) esattamente uno;  
>- (1, n) almeno uno;  
>- (0,1) opzionalmente uno;  
>- (0, n) un numero qualsiasi;  
>- (x, y) almeno x, al massimo y;  
>- (x, x) precisamente x.


%%[[000 Indice BD|↖ Ritorna all'indice ↖]]%%