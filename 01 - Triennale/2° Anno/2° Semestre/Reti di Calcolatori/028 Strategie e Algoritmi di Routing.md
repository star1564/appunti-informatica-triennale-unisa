---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Rete
cssclasses:
  - 
---
Per poter [[022 Livello di Rete#Instradamento (Routing)|instradare]] i vari pacchetti, un [[027 Router e AS|Router]] ha bisogno di alcune informazioni fondamentali, ovvero:
1. l'indirizzo IP dell'host di *destinazione*;
2. l'indirizzo dei router ad esso *adiacenti*;
3. i possibili *percorsi alternativi* per poter raggiungere reti remote.

## Tabella di Routing
Una **Tabella di Routing** raccoglie le informazioni necessarie per individuare il percorso ottimale verso tutte le possibili reti. È composta dai seguenti campi:
- *Indirizzo IP di Destinazione*: è il campo più importante contenuto nella Routing Table, quando un router riceve un pacchetto dati attraverso la sua porta di `IN`, controlla nella propria tabella di routing se esiste una entry per tale destinazione, ed in caso affermativo inoltra il flusso dati nella corrispondente porta di `OUT`;

- *Metrica*: misurazione secondo un certo criterio che interviene nella caratterizzazione di un percorso per l'instradamento di pacchetti tra due nodi (definisce il comportamento dell'algoritmo di routing);

- *Indirizzo del Router di Next Hop*: è l'indirizzo del router successivo per raggiungere la rete di destinazione;

- *Interfaccia*: interfaccia del router attraverso cui deve essere instradato il pacchetto verso il next hop.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230529110729.png|700]]
>
>![[Pasted image 20230529110922.png]]

## Longest Prefix Match
Per valutare se un certo host con indirizzo $X$ appartiene ad una [[024 Network Mask e Subnetting|sottorete]] con indirizzo $Y/M$, si effettua un'operazione di matching svolgendo 
$$X\ and\ M = Y\ and\ M$$

Se il matching darà *esito positivo* per più righe nella tabella di routing, si attua la regola del **Longest Prefix Match**, cioè si utilizza la riga con il maggior numero di bit in comune $X\ and\ M$. 

Quindi, sostanzialmente, dato un pacchetto con indirizzo di destinazione, viene effettuato il *matching con tutti gli indirizzi IP* nella tabella di routing. Se la destinazione coincide con molteplici indirizzi della tabella di routing, allora tali indirizzi avranno maschere differenti. Verrà, quindi, scelto quello con maschera maggiore grazie al Longest Prefix Match e si proseguirà con la [[026 Routing e Forwarding#Protocollo ARP|procedura ARP]].

## Routing Statico e Dinamico
Il routing può essere sia statico che dinamico:
- Nel routing **Statico**, l'amministratore di rete *configura manualmente le tabelle di routing* nei router, specificando i percorsi statici che i pacchetti devono seguire;
	- È *ingestibile* in condizioni di reti complesse.
- Nel routing **Dinamico**, invece, i router *scambiano informazioni di routing tra di loro* utilizzando protocolli di routing e aggiornano automaticamente le loro tabelle di routing in base alle informazioni ricevute.

## Algoritmi di Routing
Esistono diversi **Algoritmi di Routing** utilizzati dai router per prendere decisioni sul percorso da seguire. 

### Basati sul Percorso più Breve (Distance Vector)
Uno degli algoritmi di routing basati sul percorso più breve è *OSPF* (**Open Shortest Path First** di Dijkstra): tale algoritmo mantiene in una tabella la più piccola distanza conosciuta per ogni destinazione e quale canale utilizzare per raggiungerla. Tali tabelle vengono aggiornate scambiando informazioni con i router vicini. Questo è un algoritmo [[011 Algoritmi Greedy|greedy]].

L'[[019 Algoritmo di Dijkstra|algoritmo di Dijkstra]] utilizza il protocollo *distance vector*: l'idea è quella di partire dal nodo sorgente e di guardare i nodi adiacenti assegnando loro il valore del costo per raggiungerli.

> [!info]- Come funziona OSPF
> Supponiamo di avere questa rete con protocollo OSPF:
> ![[Pasted image 20230602092613.png]]
> 1. **Identificazione degli adiacenti**: I router OSPF identificano gli altri router OSPF direttamente collegati sulla stessa rete. 
> 	- Ciò avviene tramite l'invio di *pacchetti di hello OSPF* a intervalli regolari.
> 2. **Scambio di informazioni di topologia**: i router OSPF scambiano informazioni sulla topologia di rete utilizzando pacchetti di stato di collegamento (**LSA**, *Link State Advertisement*). 
> 	- Ognuno di questi pacchetti contiene informazioni su un router, come i sui collegamenti diretti (link, interfacce), indirizzi IP, metriche di costo e stato del collegamento (link state). 
> 	- Ogni router OSPF crea un *database di tipologia locale* dove conservare tutti gli LSA scoperti attraverso l'invio di pacchetti di hello OSPF. 
> 	- Ogni volta che due router diventano "adiacenti" (quando sono disposti a condividere tra di loro informazioni), questi avranno gli stessi elementi nel loro database.
> 	- Queste informazioni verranno propagate sull'intera *zona* di rete attraverso questo meccanismo (per esempio, l'LSA di R1 verrà condiviso con tutti i router dell'Area 0, ovvero R2, R3 e R4). Questo avviene per non sovraccaricare gli header dei pacchetti.
> 3. **Calcolo del percorso**: utilizzando il database di topologia locale, ogni router OSPF calcola il percorso più efficiente verso tutte le destinazioni nella rete. Questo viene fatto utilizzando l'algoritmo di routing di *Dijkstra*, che tiene conto delle metriche di costo associate a ciascun collegamento. 
> 	- Il percorso più efficiente è selezionato in base alla *metrica di costo minore*.
> 4. **Creazione della tabella di routing**: dopo aver calcolato il percorso più efficiente, ogni router OSPF crea la propria tabella di routing contenente anche il *distance vector*.
> 5. **Scambio di aggiornamenti di routing**: i router OSPF si scambiano periodicamente *aggiornamenti di routing (compreso il distance vector)* per mantenere le informazioni sulla topologia della rete aggiornate. Ciò avviene inviando pacchetti di stato di collegamento (LSU, Link State Update) che contengono informazioni sui collegamenti di rete.
> 6. **Convergenza di routing**: OSPF è progettato per raggiungere la convergenza di routing rapidamente. 
> 	- La *convergenza* si verifica quando tutti i router OSPF hanno calcolato i percorsi più efficienti e hanno aggiornato le loro tabelle di routing di conseguenza. Ciò consente ai pacchetti di essere inoltrati attraverso la rete in modo efficiente.

> [!question] [Come funziona il distance vector](https://www.youtube.com/watch?v=00AAnwgl2DI)

### Basati sul Protocollo Flooding
Il **Flooding** è un protocollo di instradamento usato dal router che inoltra un pacchetto in ingresso *su tutte le linee* ad eccezione di quella da cui proviene e viene solitamente usato per trovare il percorso migliore. Tale algoritmo genera un vasto numero di pacchetti duplicati, raggiungendo anche l'infinito, quindi si associa un contatore al fine di evitare ciò: se il contatore raggiunge lo 0, il pacchetto viene eliminato.

Gli aspetti negativi di questo algoritmo sono legati alla sua *inefficienza*, siccome manda ogni pacchetto su ogni rete provocando un utilizzo inefficiente della rete. Gli aspetti positivi riguardano il fatto che non c'è bisogno della conoscenza della topologia della rete che il pacchetto attraversa e che *ogni pacchetto arriverà* nel minor tempo possibile (siccome segue tutte le strade, quindi anche la più veloce).

Tale protocollo viene scarsamente utilizzato per via della sua inefficienza.

### Basati sul Link State
Gli algoritmi **Routing Link State** nascono con l'intenzione di sostituire gli algoritmi Distance Vector e si basano sull'invio di pacchetti detti **Link State Packet** (*LSP*) contenenti le *informazioni di costo* e di *ritardo* di ogni link uscente dal nodo su cui si opera. 

La propagazione degli LSP avviene tramite flooding. Ogni nodo utilizza queste informazioni per *calcolare il costo minimo verso tutti i nodi*. Quindi, sostanzialmente, il router operante associa ad ogni destinazione un costo dipendente dalla linea (link) che collega i due nodi adiacenti.

Una volta ottenute le informazioni da tutti gli altri router della rete, si costruisce il grafo di rete e si utilizza Dijkstra per trovare il cammino minimo.

Gli algoritmi LSP *non* possono gestire qualsiasi rete di qualsiasi dimensione, quindi occorre realizzare il routing in modo *gerarchico*, suddividendo la rete in *aree*.

___
[[000 Indice RC|↖ Ritorna all'indice ↖]]
```dataviewjs
/*
function extractUpperCaseLetters(inputString) {
	// Use a regular expression to match uppercase letters (A-Z)
	const uppercaseLetters = inputString.match(/[A-Z]/g);
	
	// Check if uppercaseLetters is not null, and join the matched letters into a string
	if (uppercaseLetters !== null) {
		return uppercaseLetters.join('');
	} else {
	    // If no uppercase letters were found, return an empty string
	    return '';
	}
}
*/

function extractNumberFromString(inputString) {
	const match = inputString.match(/^\d{3}/);
	
	if (match) {
		return match[0];
	} else {
		return null; // Return null if no match is found
	}
}

function startsWithNumber(inputString, targetNumber) {
  // Use a regular expression to check if the string starts with the target number
  const pattern = new RegExp(`^${targetNumber}`);
  return pattern.test(inputString);
}

function sort2Array(array){
	let res = []
	
	if (array[0] == undefined)
		res.push(array[1], array[1])
	else if (array[1] == undefined)
		res.push(array[0], array[0])
	else if (parseInt(extractNumberFromString(array[0])) > parseInt(extractNumberFromString(array[1])))
		res.push(array[1], array[0])
	else
		res.push(array[0], array[1])
	
	return res
}

let toDisplay = []
function searchPrevAndNext(tag, currentNumber) {
	const prevNumber = (parseInt(currentNumber) - 1).toString().padStart(3, "0");
	const nextNumber = (parseInt(currentNumber) + 1).toString().padStart(3, "0");
	
	let prevAndNext = [(dv.pages(tag).where(p => startsWithNumber(p.file.name, prevNumber) || startsWithNumber(p.file.name, nextNumber)).file.name)]
	
	// Prev = [0]; Next = [1]
	let sortedPrevAndNext = sort2Array(prevAndNext[0])
	
	if (currentNumber == "001"){ 
		// Se è la prima pagina aggiungi solo il prossimo
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
	} else if (prevAndNext[0][1] == undefined){
		// Se è l'ultima pagina aggiungi solo il precedente
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	} else {
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva →" + "]]"
		toDisplay.push(nextLink)
		
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	}
	
	
}

if (dv.current().tags[0] == null || dv.current().tags[0] == undefined){
	dv.header(1, "Errore - Inserire il tag nelle proprietà del file")
} else {
	let tag = "#" + dv.current().tags[0]

	// Purtroppo obsidian non riesce a leggere i link e a creare connessioni nel grafo,
	// quindi ho disattivato il link all'indice automatico e la funzione extractUpperCaseLetters
	//let indexName = "000 Indice " + extractUpperCaseLetters(tag)
	//let index = dv.page(indexName).file
	//let indexLink = "[[" + index.name + "|" + "↖ Ritorna all'indice ↖" + "]]"
	//toDisplay.push(indexLink)
	
	let currentPage = dv.current().file
	let currentPageNumber = extractNumberFromString(currentPage.name)
	
	searchPrevAndNext(tag, currentPageNumber)
	
	dv.list(toDisplay)
}
```
Altri collegamenti: 
- [Cisco - OSPF](https://www.cisco.com/c/it_it/support/docs/ip/open-shortest-path-first-ospf/7039-1.html)
- [What is OSPF and How Does It Work?](https://www.youtube.com/watch?v=Xb3CbIDMDRk)