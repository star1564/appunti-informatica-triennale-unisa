---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
Esistono generalmente tre tipologie in cui può accadere un *loop* quando si connettono due o più [[016 Bridge e Switch#Switch|switch]].
I possibili loop si possono prevenire attraverso lo [[018 Spanning Tree Protocol|Spanning Tree Protocol]].

A partire dal [[Protocollo]] IPv4, è possibile individuare pacchetti che vanno in loop attraverso il *Time To Live* (TTL), utilizzato per prendere azione.

### Broadcast Storm
![[Pasted image 20230426103355.png|500]]
1. L'host X manda un [[002 Rappresentazione di Reti#Broadcast|broadcast]];
2. Lo switch A replica il Broadcast su tutte le sue porte (anche quella verso lo switch B);
3. Lo switch B replica il Broadcast su tutte le sue porte (anche quella verso lo switch A);
4. Si ripetono questi due passaggi all'infinito, creando una **Broadcast Storm**.

### Replicazione della Trama
Tecnicamente questo non è un loop, ma è lo stesso dannoso per una rete.
La destinazione ottiene pacchetti duplicati.

![[Pasted image 20230426103657.png|500]]

1. L'host X invia una trama unicast al router Y;
2. L'[[MAC (Indirizzo e Sottolivello)#Indirizzo MAC|indirizzo MAC]] del Router Y non è stato “imparato” da nessuno dei 2 switches;
3. Entrambi gli switch inviano il messaggio su tutte le porte;
4. Il Router Y riceve 2 copie della medesima trama.

### Instabilità del MAC address database
![[Pasted image 20230426104021.png|500]]
1. L'host X invia una trama unicast al router Y;
2. L'indirizzo MAC del Router Y non è stato “imparato” da nessuno dei 2 switches;
3. Gli Switch A and B vedono il MAC address di X sulla porta 0;
4. La trama diretta al router Y è inviata a tutte le porte;
5. Gli switch A and B vedono in maniera non corretta il MAC address di X sulla porta 1

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