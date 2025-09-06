---
tags:
  - corsi/informatica/reti_calcolatori
  - esercizi
---
Si debba suddividere la rete 191.204.0.0 in sottoreti aventi ciascuna un massimo di 14 host. Quale netmask utilizzerete? Quante subnet si ottengono? Indicare il ragionamento.

La rete in questione è di classe B. Per suddividerla in sottoreti aventi ciascuna un massimo di 14 host, occorre riservare al più gli ultimi 4 bit di ciascun indirizzo al campo host, il che impone nell'ultimo byte un campo subnet di 12 bit. Il numero di bit necessari a indicare l'insieme rete + sottorete è dunque $16 + 12 = 28$ bit. 

La netmask che si individua è di conseguenza 255.255.255.240 (in binario 11111111 11111111 11111111 11110000), e consente di suddividere la rete di classe B in $2^{12}=4096$ subnet (RFC 1878).

![[Pasted image 20230529174840.png]]

%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%