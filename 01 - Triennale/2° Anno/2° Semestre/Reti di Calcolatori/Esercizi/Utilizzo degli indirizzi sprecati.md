---
tags:
  - corsi/informatica/reti_calcolatori
  - esercizi
---
>Supponiamo di avere un ***netblock*** (non si può oltre il range [211.2.17.0, 211.2.17.255]) di classe C: 211.2.17.0/24.
>Si vogliono creare cinque sottoreti in grado di contenere almeno i seguenti host come di seguito elencato:
>- A: 22
>- B: 24
>- C: 78
>- D: 28
>- E: 25

> [!tip]+ Calcoli veloci per un singolo netblock
> Se si devono fare calcoli solo su un netblock, basta prendere $2^x-1$  e sommarlo al gateway attuale per ottenere il broadcast attuale. Poi alla prossima sottorete, il gateway sarà il gateway precedente + 1. Per esempio:
> C: 211.2.17.0 ($2^7-1=128-1=127$), $211.2.17.0+0.0.0.127=211.2.17.127$
> A: 211.2.17.128 ($2^5-1=32-1=31$), $211.2.17.128+0.0.0.31=211.2.17.159$
> ...


Abbiamo i seguenti range di indirizzi (dove le sottoreti A, B, D, E utilizzano lo stesso numero di bit per rappresentare la parte della rete negli indirizzi, quindi il loro ordine non conta):
- Rete C: 78 host, $2^7-2=128-2=126\geq78$, $32-24-7=1\ bit$, $24+1=/25$
	- Gateway C:   211.2.17.0/25
	- Broadcast C: 211.2.17.127
- Rete A, B, D, E: {22,24,28,25} host, $2^5-2=32-2=30\geq A,B,D,E$, $32-25-5=2\ bit$, $25+2=/27$
	- Gateway A:   211.2.17.128/27
	- Broadcast A: 211.2.17.159
	- Gateway B:   211.2.17.160/27
	- Broadcast B: 211.2.17.191
	- Gateway D:   211.2.17.192/27
	- Broadcast D: 211.2.17.223
	- Gateway E:   211.2.17.224/27
	- Broadcast E: 211.2.17.255


>Supponendo che la sottorete D passi da 28 a 36 host, suggerire le modifiche da apportare.

Se la sottorete D passa da 28 a 36 host, lo spazio di indirizzamento utilizzato non è più sufficiente. Inoltre non è possibile allocare altre sottoreti, perché lo spazio è saturo (si noti che il broadcast della rete E è 211.2.17.255, un ulteriore netblock dovrebbe avere un gateway al di fuori del netblock dato dalla traccia).

Tuttavia, calcolando il numero di indirizzi sprecati da ogni sottorete, possiamo notare che la rete C spreca 50 indirizzi dei 128 allocati.
Possiamo rendere C come due reti contigue C1 e C2 da 64 e 32 host rispettivamente. Effettuando la divisione di C in C1 e C2, recuperiamo un netblock di 32 hosts.

![[Pasted image 20230530103253.png|700]]

Pertanto:
- Rete C1: 64 host
	- Gateway C1:   211.2.17.0/26
	- Broadcast C1: 211.2.17.63
- Rete C2: 32 host
	- Gateway C2:   211.2.17.64/26
	- Broadcast C2: 211.2.17.95

A questo punto, per la rete D, si accorcia la netmask di 1 bit. Tale operazione consentirà alla rete D di utilizzare si agli indirizzi allocati in precedenza, sia i 32 indirizzi liberati dal partizionamento di C.

Pertanto: Rete D, 36 host: Gateway D: 211.2.17.192/26

%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%