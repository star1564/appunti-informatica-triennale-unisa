---
tags:
  - corsi/informatica/reti_calcolatori
---
### Indirizzo MAC
>Un **MAC Address** (Media Access Control address) è un *identificatore univoco a livello fisico* associato alla scheda di rete (**NIC**) di un dispositivo, come un computer o uno smartphone, che lo collega a una rete.

Ogni indirizzo MAC è composto da sei coppie di caratteri esadecimali. $$FF-FF-FF-FF-FF-FF$$
Il MAC è sempre **connectionless**, quindi *non si occupa della correzione degli errori riguardo la trasmissione*. In ogni caso, le LAN sono reti sicure ed affidabili, quindi non è necessario controllare e correggere errori. Se ciò fosse richiesto, se ne occuperebbe il sottolivello [[LLC]].

> [!question]+ Come si determina l'indirizzo MAC se si conosce solo l'[[Indirizzo IP]] di un nodo?
> Ogni nodo IP nella [[LAN]] ha una *tabella ARP*, contenente la corrispondenza tra indirizzi IP e MAC.

![[Indirizzo IP#^4b2008]]

---
### Sottolivello MAC
>Il **[[015 La Rete Ethernet#^34aead|Sottolivello]] MAC** è specifico di ogni LAN e risolve il problema della [[011 Collegamenti di rete nel Data Link#Protocolli ad accesso multiplo|condivisione]] del mezzo trasmissivo.

In *trasmissione*, sceglie chi può usare il canale; in *ricezione* determina a quali sistemi è destinato un certo pacchetto.


%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%