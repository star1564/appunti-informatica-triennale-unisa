---
tags:
  - corsi/informatica/reti_calcolatori
  - esercizi
---
![[Pasted image 20230530165620.png|1000]]

Per vedere l'instradamento di un pacchetto raggruppiamo gli indirizzi IP secondo la loro netmask indicando anche il next hop:

NETMASK $255.255.\textcolor{#00C575}{254}.0$:
- $131.98.170.0$, $E0$
- $131.98.168.0$, $E1$
- $131.98.166.0$, $S2$

NETMASK $255.255.\textcolor{#FF6611}{252}.0$:
- $131.98.164.0$, $S3$

NETMASK $0.0.0.0$ (default)
- $0.0.0.0$ (default), $S4$

> [!tip] Instradamento diretto e indiretto
> Se si utilizza la tabella di routing, allora si esegue un instradamento *indiretto*.
> Se un indirizzo di destinazione **proviene da una delle interfacce** del router (eth0, eth1, ecc.), si esegue un instradamento *diretto* verso le altre interfacce (non si fa altro).


> [!note] Instradare pacchetti
>Bisogna fare l'AND bit a bit tra le netmask e gli indirizzi delle destinazioni.
>- Se l'indirizzo di destinazione è:
>	- Un pacchetto di broadcast limitato ($255.255.255.255$) → Nessun inoltro
>	- Di tipo unicast limitato ($0.0.0.x$) → Nessun inoltro	
>‎ 
>- Se si deve eseguire un instradamento diretto, mandare il pacchetto verso l'interfacca del router (dove la parte dell'host non conta), altrimenti esegui i calcoli per l'instradamento indiretto.
>‎ 
>- Se, dato l'indririzzo di destinazione $d$ e la netmask $M$, si ottiene da $d\ AND\ M$ un indirizzo $i$ che si trova nel gruppo della STESSA netmask $M$, si manda il 
>- pacchetto sul next hop associato ad $i$.
>- Se non vi è l'indirizzo $i$ in NESSUNO dei gruppi delle netmask $M$, allora si manda il pacchetto sul next hop associato al default.
>- Se l'indirizzo ha una corrispondenza in netmask diverse, si sceglie il gruppo con la netmask con il prefisso più lungo.

(a) 
$131.98.171.92\ AND\ 255.255.\textcolor{#FF6611}{252}.0=131.98.\textcolor{#CC241D}{168}.0$        NON SI TROVA NEL GRUPPO DELLA NETMASK $255.255.\textcolor{#FF6611}{252}.0$

$131.98.171.92\ AND\ 255.255.\textcolor{#00C575}{254}.0=131.98.\textcolor{#CC241D}{170}.0$        UNICA CORRISPONDENZA IN $255.255.\textcolor{#00C575}{254}.0$, VIENE MANDATO SU $E0$ ◀

(b) 
$131.98.167.151\ AND\ 255.255.\textcolor{#FF6611}{252}.0=131.98.\textcolor{#CC241D}{164}.0$        CORRISPONDENZA IN $255.255.\textcolor{#FF6611}{252}.0$

$131.98.167.151\ AND\ 255.255.\textcolor{#00C575}{254}.0=131.98.\textcolor{#CC241D}{166}.0$        CORRISPONDENZA IN $255.255.\textcolor{#00C575}{254}.0$

Si sceglie l'indirizzo con la netmask con il prefisso più lungo:
$255.255.\textcolor{#00C575}{254}.0$ ha il prefisso più lungo ($23\ bit > 22\ bit$), quindi viene mandato su $S2$. ◀

(c)
$131.98.163.151\ AND\ 255.255.\textcolor{#FF6611}{252}.0=131.98.\textcolor{#CC241D}{160}.0$        NON SI TROVA NEL GRUPPO DELLA NETMASK $255.255.\textcolor{#FF6611}{252}.0$

$131.98.163.151\ AND\ 255.255.\textcolor{#00C575}{254}.0=131.98.\textcolor{#CC241D}{162}.0$        NON SI TROVA NEL GRUPPO DELLA NETMASK $255.255.\textcolor{#00C575}{254}.0$

Non ci sono corrispondenze per nessun gruppo di indirizzi, quindi lo si manda sul default $S4$. ◀

(d)
$131.98.169.192\ AND\ 255.255.\textcolor{#FF6611}{252}.0=131.98.\textcolor{#CC241D}{168}.0$        NON SI TROVA NEL GRUPPO DELLA NETMASK $255.255.\textcolor{#FF6611}{252}.0$

$131.98.169.192\ AND\ 255.255.\textcolor{#00C575}{254}.0=131.98.\textcolor{#CC241D}{168}.0$        UNICA CORRISPONDENZA IN $255.255.\textcolor{#00C575}{254}.0$, VIENE MANDATO SU $E1$ ◀

(e)
$131.98.165.121\ AND\ 255.255.\textcolor{#FF6611}{252}.0=131.98.\textcolor{#CC241D}{164}.0$        UNICA CORRISPONDENZA IN $255.255.\textcolor{#FF6611}{252}.0$, VIENE MANDATO SU $S3$ ◀

$131.98.165.121\ AND\ 255.255.\textcolor{#00C575}{254}.0=131.98.\textcolor{#CC241D}{164}.0$        NON SI TROVA NEL GRUPPO DELLA NETMASK $255.255.\textcolor{#00C575}{254}.0$

%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%