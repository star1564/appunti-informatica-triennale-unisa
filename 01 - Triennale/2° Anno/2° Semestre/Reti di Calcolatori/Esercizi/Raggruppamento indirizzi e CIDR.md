---
tags:
  - corsi/informatica/reti_calcolatori
  - esercizi
---
![[Pasted image 20230530111547.png]]

> [!note] Gruppi CIDR
> Per verificare se un gruppo è un gruppo CIDR, bisogna verificare se gli indirizzi hanno i primi $n$ bit in comune, dove $n$ è la lunghezza della netmask.

Iniziamo a raggruppare gli IP in base alla netmask.

NETMASK: $255.255.255.224$:
- $200.150.150.161$
- $200.150.150.190$
- $200.150.150.180$
- $200.150.150.151$
- $200.150.150.192$
- $200.150.150.182$
- $200.150.150.162$
- $200.150.150.152$

NETMASK: $255.255.240.0$:
- $150.150.150.150$
- $150.150.134.145$
- $150.150.134.154$
- $150.150.134.158$

NETMASK: $255.255.252.0$:
- $200.150.150.151$
- $200.150.150.150$
- $200.150.151.150$
- $200.150.151.151$

A questo punto calcoliamo il numero di subnet indirizzate da ogni netmask identificata. Il calcolo delle subnet, e quindi dei loro range di indirizzi, serve a controllare la presenza di eventuali sottogruppi.

Per la netmask $255.255.255.224$ abbiamo 8 reti da 32 hosts.
Consideriamo i vari intervalli, ottenendo i seguenti gruppi:
![[Pasted image 20230530112248.png]]

Per la netmask $255.255.240.0$ abbiamo i seguenti gruppi:
![[Pasted image 20230530112317.png]]

Per la netmask $255.255.252.0$, tutto resta invariato.

A questo punto, per ogni blocco, testiamo se si tratta di un gruppo CIDR. 
Il test darà come risultato il gruppo associato alla netmask $255.255.252.0$.

%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%