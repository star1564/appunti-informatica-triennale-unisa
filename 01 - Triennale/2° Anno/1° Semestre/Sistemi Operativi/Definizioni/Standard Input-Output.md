---
tags:
  - corsi/informatica/sistemi_operativi
---
All'esecuzione di un comando, si crea un processo. Questo va ad aprire tre flussi:
- `stdin`, detto *ingresso standard* (standard input), nel quale il processo leggerà i dati in entrata. Di default corrisponde alla tastiera;
- `stdout`, detto *uscita standard* (standard output), nel quale il processo scriverà i dati in uscita. Di default corrisponde allo schermo;
- `stderr`, detto *errore standard* (standard error), nel quale il processo scriverà i messaggi di errore. Di default corrisponde alla schermo.

![[Pasted image 20220924131335.png]]

---
#### RIDIREZIONE OUTPUT
Linux ha dei meccanismi che permettono di **Reindirizzare** le I/O Standard verso dei file. 
Questo accade attraverso l'utilizzo del carattere `>`, che permette di reindirizzare l'uscita standard di un comando posto a sinistra verso il file posto a destra.

```C
ls -al /home/nome_utente/ > lista.txt  
echo "lista" > /etc/file_di_configurazione
```

Il comando di sopra è equivalente ad una copia di file.

[[000 Indice SO|↖ Ritorna all'indice ↖]]