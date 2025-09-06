---
tags:
  - corsi/informatica/sistemi_operativi
---
Un **Interrupt** è un segnale che indica un *nuovo evento* e quindi il "bisogno di attenzione" da parte di una periferica finalizzata ad una particolare richiesta di servizio.

Ogni tipo di interrupt ha un suo codice con cui si determinano le azioni da intraprendere per gestire l'evento attraverso la *routine di gestione dell'interrupt*.

Gli interrupt sono solitamente disabilitati mentre si sta eseguendo una routine di gestione di un altro interrupt, al fine di prevenire la perdita di interrupt.

Una **Trap** è un interrupt generato dal software, causato da un errore durante la computazione oppure da una richiesta specifica dell'utente. ^4b8f83

## MECCANISMO DELLE INTRODUZIONI
Quando la CPU riceve un interrupt, *sospende* ciò che sta facendo e *comincia ad eseguire* codice a partire da una locazione fissa, che contiene l'indirizzo di partenza della routine di interrupt.

![[Pasted image 20220921115002.png|700]]

L'architettura dell'interrupt trasferisce il controllo alla routine di gestione attraverso il *vettore degli interrupt*, che contiene gli indirizzi di tutte le routine di servizio.
Si salva l'indirizzo dell'istruzione interrotta e lo stato del processore (come quello dei registri) in un'area di memoria chiamata *stack*.


%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%