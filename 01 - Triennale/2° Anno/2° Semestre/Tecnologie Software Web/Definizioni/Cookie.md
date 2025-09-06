---
tags:
  - corsi/informatica/tecnologie_software_web
---
Parallelamente alle sequenze [[004 Richiesta e Risposta HTTP#RISPOSTA|request/response]], il protocollo prevede una struttura dati che si muove come un token, dal [[Client]] al [[Server]] e viceversa: i **cookie**.

Hanno come scopo quello di fornire un supporto per **[[014 Session Tracking#Cookie|il mantenimento di stato]]** in un protocollo come [[003 HTTP|HTTP]] (che è essenzialmente stateless).

>Un **cookie** è un "client-side storage" che, dopo la richiesta di una risorsa, *viene mandato insieme alla risposta dal server e salvato nel browser*.
>Dopo la sua creazione viene sempre passato ad ogni trasmissione con quel server per le request e response.

I tipici utilizzi di un cookie potrebbero essere: 
- il salvataggio delle preferenze dell'utente (ad esempio l'attivazione della modalità scura del sito);
- l'identificazione di un utente in una sessione di e-commerce;
- l'autenticazione (saltando l'inserimento di un username e password);
- l'implementazione di focused advertising.

![[Pasted image 20230302171606.png|600]]

Solo il sito che fa creare/crea il cookie può rileggerlo, inoltre ha una scadenza specificata dal sito.

%%
[[000 Indice TSW|↖ Ritorna all'indice ↖]]
%%