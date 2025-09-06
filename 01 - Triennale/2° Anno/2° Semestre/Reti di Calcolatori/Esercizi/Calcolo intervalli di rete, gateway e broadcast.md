---
tags:
  - corsi/informatica/reti_calcolatori
  - esercizi
---
Si assegni lo spazio di indirizzamento IP necessario a ciascuna delle sottoreti rappresentata in figura. Si assuma di voler minimizzare lo spazio degli indirizzi da acquistare (il provider offre indirizzi compresi in 50.10.192.0/18 ma il costo per singolo indirizzo è elevato). Si elenchino, per ciascuna sottorete, gli indirizzi di rete, di broadcast e l'intervallo dei possibili indirizzi disponibili per gli host.

![[Pasted image 20230529160904.png|700]]

1. Individuare il numero di <font color="green">Sottoreti</font> presenti;
2. Individuare il <font color="yellow">numero degli host</font> per ogni <font color="green">sottorete</font> presente;
3. Vedere la classe dell'indirizzo di partenza e se la rete riesce a <font color="FF6611">soddisfare</font> la totalità degli indirizzi richiesti;
4. Partendo dalla sottorete con più host (su cui non si sono ancora svolti calcoli) e: 
	- Trovare i <font color="#61AFEF">bit della netmask precedente</font> (o quelli dati a priori);
	- Trovare la <font color="00C575">potenza</font> di 2 immediatamente successiva al numero di host e sottrargli 2;
	- Sottrare: $\textcolor{#CC241D}{r}=32-\ \textcolor{#61AFEF}{\#\ bit\ netmask\ precedente}-\textcolor{#00C575}{potenza}$. Questo è il <font color="#CC241D">numero di bit di cui si deve allungare la netmask</font>.
	- Sommare: $\textcolor{#B35DB0}q=\textcolor{#61AFEF}{\#\ bit\ netmask\ precedente}+\textcolor{#CC241D}{r}$. Questo è il <font color="#B35DB0">numero di bit pari a 1 nella netmask attuale</font> (a partire da sinistra).
	- Se non è la prima iterazione, prendere la netmask attuale ed il broadcast calcolato precedentemente (a cui gli si somma 1 in binario). Quest'ultimo è il gateway per la seguente sottorete.
		- Se è la prima iterazione, il gateway per questa sottorete sarà l'indirizzo dato a priori $/\textcolor{#B35DB0}q$.
	- A partire dal primo bit pari a 0 nella netmask attuale, convertire tutto quello che c'è alla sua destra (nel gateway attuale) e trasformalo in 1. Quello che si è ottenuto è il nuovo indirizzo di broadcast per la seguente sottorete.
	- Ripetere il passo 4 fino a quando non si finiscono le sottoreti disponibili.



---
Sono presenti 5 sottoreti.
![[Pasted image 20230529161114.png|700]]

<font color="yellow">Numero degli host</font> per ogni <font color="green">sottorete</font> presente:
- NET LAN: 70
- NET 1: 312
- NET 2: 62
- NET 3: 125
- ROUTER LAN: 3 (tre router presenti)

L'indirizzo 50.10.192.0/18 è di classe A ed i primi 18 bit rappresentano la rete. Quindi abbiamo 32-18=14 bit a disposizione per rappresentare gli indirizzi.
![[Pasted image 20230529161724.png]]
$$2^{14}-2=16.384-2=\textcolor{#FF6611}{16.382\ indirizzi\ disponibili}$$
Si sottrae 2 perché dobbiamo riservare il primo (000...000) e l'ultimo (111...111) indirizzo per il Gateway e Broadcast rispettivamente.

![[Pasted image 20230529171234.png]]
![[Pasted image 20230529171254.png]]
![[Pasted image 20230529171310.png]]
![[Pasted image 20230529171335.png]]
![[Pasted image 20230529171347.png]]

%%
[[000 Indice RC|↖ Ritorna all'indice ↖]]
%%