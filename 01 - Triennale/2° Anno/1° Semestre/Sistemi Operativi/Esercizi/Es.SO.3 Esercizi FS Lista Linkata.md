---
tags:
  - corsi/informatica/sistemi_operativi
  - esercizi
---
Alcuni esempi di esercizi su un [[013 Allocazione linkata di file|file system ad allocazione lista linkata]].

### AGGIUNGERE UN BLOCCO ALL'INIZIO
![[Pasted image 20221010200001.png]]
- Si individua il blocco libero nella bitmap;
- Si scrive nel blocco $x$ (in una sola volta) i dati da scrivere ed il puntatore al blocco iniziale della lista, in modo che quell'elemento punti a $b$;
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE IL FCB|cambia il FCB]];
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE LA BITMAP|cambia la bitmap]].

C'è stato $1$ [[Accesso al Disco]].

### AGGIUNTA IN MEZZO
![[Pasted image 20221010200735.png]]
- Calcola quale dopo quale blocco si deve inserire il nuovo: $size/2\begin{cases} size\ pari: risultato-1 \\ size\ dispari: risultato \end{cases}$;
- Individua il blocco $x$ vuoto dalla bitmap;
- Arriva al blocco calcolato prima, portando in MP e rimettendo in DISCO i nodi inutili, conserva $c$;
- Copia in $x$ i dati ed il puntatore a $d$;
- Cambia il puntatore di $c$ in $x$ e riportalo in DISCO;
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE IL FCB|cambia il FCB]];
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE LA BITMAP|cambia la bitmap]].

Ci sono stati $5$ [[Accesso al Disco|Accessi al Disco]].

### RIMUOVERE UN BLOCCO
![[Pasted image 20221010201959.png]]
- Arriva al blocco da eliminare, portando in MP e rimettendo in DISCO i nodi inutili, conservalo insieme al blocco precedente ($c$ e $d$);
- Scrivi il puntatore $e$ ottenuto da $d$ all'interno di $c$;
- Riporta $c$ al DISCO;
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE IL FCB|cambia il FCB]];
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE LA BITMAP|cambia la bitmap]].

In questo esempio ci sono stati $4$ [[Accesso al Disco|Accessi al Disco]].

%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%