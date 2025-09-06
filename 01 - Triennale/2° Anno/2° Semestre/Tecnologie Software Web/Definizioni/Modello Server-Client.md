---
tags:
  - corsi/informatica/tecnologie_software_web
---
In questo modello utilizzato dal Web, i dati sono *memorizzati* in computer ad alte prestazioni chiamati [[Server]].

Al contrario, chi vuole *accedere* ai dati sul server dispongono di macchine più semplici, chiamate [[Client]] (*Browser Web*).

![[Immagine 2023-02-27 175732 1.png]]
<center>Modello Client-Server</center>

Questa configurazione, chiamata **modello client-server**, è ampiamente utilizzata, soprattutto per l'implementazione di un'*applicazione Web*.
In esso, il server genera (a partire dal suo database) le pagine Web in risposta a *richieste* da parte del client che potrebbero, a loro volta, aggiornare il database stesso.

Se guardiamo in dettaglio il modello client-server vediamo che sono coinvolti due [[Processo (Job)|Processi]], uno sulla macchina client e uno sulla macchina server. 
La comunicazione è rappresentata da un processo client che *manda un messaggio* attraverso la rete al processo server e resta in attesa di un messaggio di risposta. 
Quando il processo server *riceve la richiesta*, esegue il lavoro o recupera i dati desiderati e restituisce una risposta.

![[Pasted image 20230227181235.png]]

Nella maggior parte delle situazioni un server può soddisfare le richieste di un gran numero (centinaia o migliaia) di client contemporaneamente.

%%
[[000 Indice TSW|↖ Ritorna all'indice ↖]]
%%