---
tags:
  - corsi/informatica/sistemi_operativi
  - esercizi
---
Alcuni esempi di esercizi su un [[014 Allocazione indicizzata di file|file system ad allocazione indicizzata]].

### AGGIUNTA DI UN BLOCCO ALLA FINE
![[Pasted image 20221011093819.png]]
Aggiungi $x$ alla fine di $g$.

- Trovo un blocco libero $x$;
- Porto alla MP $e$ (dato che ne ho il puntatore nel FCB);
- Ricavo il puntatore $g$ da $e$;
- Riporto $e$ nel DISCO;
- Porto alla MP $g$ ed $x$;
- Inserisco alla fine di $g$ il puntatore $x$ ed inserisco i dati nel blocco $x$;
- Riporto $g$ nel DISCO;
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE IL FCB|cambia il FCB]];
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE LA BITMAP|cambia la bitmap]].

Ci sono stati $5$ [[Accesso al Disco|Accessi al Disco]].

### RIMOZIONE DI UN BLOCCO ALLA FINE
Rimuovi l'ultimo blocco di $g$.

- Porto alla MP $e$ (dato che ne ho il puntatore nel FCB);
- Ricavo il puntatore $g$ da $e$;
- Riporto $e$ nel DISCO;
- Porto alla MP $g$;
- Tolgo il puntatore dell'ultimo blocco da $g$;
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE LA BITMAP|cambia la bitmap]];
- Si [[Es.SO.1 Modificare Bitmap-Lista ed il FCB#MODIFICARE IL FCB|cambia il FCB]];
- Riporto $g$ nel DISCO.

Ci sono stati $4$ [[Accesso al Disco|Accessi al Disco]].





%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%