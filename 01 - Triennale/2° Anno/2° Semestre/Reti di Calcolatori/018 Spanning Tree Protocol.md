---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
> Lo **Spanning Tree Protocol** è un [[Protocollo]] che costruisce una topologia locale di una [[015 La Rete Ethernet|rete Ethernet]] senza [[017 Switching Loop|loop]] (*loop free*) utilizzando la struttura ad [[023 Alberi Binari#ALBERI|albero]].

Quando una interfaccia (come le porte di un [[016 Bridge e Switch|Bridge o uno Switch]]) è connessa ad una rete, lo spanning tree comincia un processo in cui *identifica* se su quell'interfaccia *può verificarsi un loop*.

Per capire questo, costruisce una sua topografia ad albero *rimanendo in ascolto* per un certo periodo di tempo e fare il [[016 Bridge e Switch#Backward learning|backward learning]].

Ha inoltre bisogno di individuare il **nodo radice della rete** (generalmente nell'albero si trovano in alto a tutto).

Ci sono tre modalità in cui si potrà trovare una porta:
- Modalità <font color="#df1e09">Blocked Port</font>: una interfaccia accesa verrà impostata a questa modalità se potrebbe accadere un loop, bloccando tutto il traffico (sia in entrata che e uscita).

- Modalità <font color="#0c64c3">Root Port</font>: una interfaccia accesa verrà impostata a questa modalità per indicare che questa è la più vicina al nodo radice della rete (generalmente nell'albero sono quelle che vanno dal basso verso l'alto); può passare traffico normalmente.

- Modalità <font color="#5ac38f">Designated Port</font>: una interfaccia accesa in cui può passare traffico normalmente.

![[Immagine 2023-04-26 113059.png|800]]

Per esempio, se C vuole comunicare con Y, non potrà passare attraverso il Bridge 11. La porta è stata bloccata perché altrimenti si poteva scatenare un loop. Quindi dovrà seguire questo percorso C→21→1→6→5→Y.

> [!question]+ Come individuare una porta da bloccare?
> Solitamente si individua una Blocked Port determinando il *percorso meno costoso* da ciascun nodo di rete / switch al root bridge.
> 
> ![[Pasted image 20230426113953.png|400]]
> 
> In questo caso gli switch X e Y hanno la stessa priorità, ma due [[MAC (Indirizzo e Sottolivello)#Indirizzo MAC|indirizzi MAC]] diversi, quindi si chiude la porta con l'indirizzo più grande.

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