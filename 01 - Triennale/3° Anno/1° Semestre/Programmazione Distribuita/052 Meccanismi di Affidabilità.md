---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Message Service
cssclasses:
  - 
---
>[[049 Java Message Service|JMS]] definisce diversi **Meccanismi di Affidabilità** per *garantire la consegna dei messaggi*, anche se il provider è crashato, è sotto carico, o se le destinazioni sono piene di messaggi che dovrebbero essere scaduti.

I meccanismi di affidabilità sono i seguenti:
- **Filtrare i messaggi**: utilizzando i selettori (con sintassi simile a SQL) è possibile *filtrare i messaggi che si desiderano ricevere per un certo consumer*. Questo in modo da ridurre il tempo e banda per inviare messaggi inutili per certi consumer;

```Java
// Specifica i filtri per tre consumer
MessageConsumer c1 = context.createConsumer(queue, "JMSPriority < 6").receive();
MessageConsumer c2 = context.createConsumer(queue, "JMSPriority < 6 AND orderAmount < 200").receive();
MessageConsumer c3 = context.createConsumer(queue, "orderAmount BETWEEN 1000 AND 2000").receive();

// Crea due messaggi con certe proprietà
context.createTextMessage().setIntProperty("orderAmount", 1530);
context.createTextMessage().setJMSPriority(5);
```

- **Impostazione del time-to-live dei messaggi**: impostare un tempo di scadenza per i messaggi, in modo che non vengano consegnati se sono obsoleti;

```Java
context.createProducer().setTimeToLive(1000).send(queue, message);
```

- **Specificare la persistenza dei messaggi**: specificare la persistenza dei messaggi in caso di guasto del provider. La *consegna persistente* assicura che un messaggio sia consegnato *solo una volta* a un consumatore, mentre la *consegna non persistente* richiede che un messaggio sia consegnato *al massimo una volta* (potrebbe anche non arrivare);

```Java
context.createProducer().setDeliveryMode(DeliveryMode.NON_PERSISTENT).send(queue, message);
```

- *Controllo dell'acknowledgment*: specificare vari livelli di acknowledgment dei messaggi;
- *Creare subscription durevoli*: assicurare che i messaggi siano consegnati a un subscriber non disponibile in un [[050 Tipi di destinazioni per Messaggi|modello pub-sub]],
- *Impostazione delle priorità*: Impostare la priorità di consegna di un messaggio.


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