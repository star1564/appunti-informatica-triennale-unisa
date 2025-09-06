---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Enterprise Java Bean
cssclasses:
  - 
---
>Una **Transazione** viene utilizzata per *garantire che i dati siano mantenuti in uno stato coerente*. Rappresenta un gruppo logico di operazioni che devono essere eseguite *come una singola unità*, nota anche come unità di lavoro. 
>
>Ogni operazione *deve avere successo* affinché la transazione abbia successo (si dice che la transazione è **committed**). Se una delle operazioni *fallisce*, anche la transazione fallisce (la transazione viene annullata con un **roll-back**).

^991e46

Queste operazioni, che vengono eseguite in sequenza o in parallelo in un periodo di tempo relativamente breve, possono comportare la [[033 Java Persistence API|persistenza]] dei dati in uno o più database, l'invocazione di [[049 Java Message Service|messaggi]] o [[053 Web Service|Servizi Web]].

Le transazioni devono garantire un certo grado di affidabilità e robustezza e seguire le proprietà [[ACID]].

### Condizioni di Lettura
L'isolamento delle transazioni può essere definito utilizzando diverse **condizioni di lettura**. Esse descrivono *cosa può accadere quando due o più transazioni operano contemporaneamente sugli stessi dati*. A seconda del livello di isolamento che si adotta, si può evitare del tutto o consentire l'[[011 Metodi Sincronizzati|accesso concorrente]].

- **Dirty reads**: si verificano quando una transazione *legge le modifiche non committed* apportate dalla transazione precedente.
- **Repeatable reads**: si verifica quando i dati letti *saranno gli stessi se letti di nuovo* durante la stessa transazione.
- **Phantom reads**: si verificano quando i nuovi record aggiunti al database *sono rilevabili da transazioni iniziate prima dell'inserimento*. Le query includeranno i record aggiunti da altre transazioni dopo l'avvio della loro transazione.

### Livelli di Isolazione di una Transazione
I database utilizzano diverse tecniche di blocco per *controllare il modo in cui le transazioni accedono ai dati in modo concorrente*. I meccanismi di blocco hanno un impatto sulle condizioni di lettura. I quattro tipi di livelli di isolamento sono:
- **Read uncommitted** (livello di isolamento meno restrittivo): la transazione *può leggere dati non committed*. 
	- È possibile effettuare dirty, repeatable e phantom reads.
- **Read committed**: la transazione *non può leggere i dati non committed*. 
	- Le dirty read sono impedite, ma non quelle repeatable o phantom.
- **Repeatable read**: la transazione *non può modificare i dati letti da un'altra transazione*. 
	- Le dirty e repeatable reads sono impedite, ma possono verificarsi phantom reads.
- **Serializable** (livello di isolamento più restrittivo): la transazione *ha una lettura esclusiva*. 
	- Le altre transazioni non possono né leggere né scrivere gli stessi dati.

In generale, man mano che il livello di isolamento diventa più restrittivo, le prestazioni del sistema diminuiscono perché le transazioni non possono accedere agli stessi dati. Tuttavia, il livello di isolamento impone la coerenza dei dati.

___
[[000 Indice PD|↖ Ritorna all'indice ↖]]

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