---
aliases:
  - Ethernet
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Data Link
cssclasses:
  - 
---
> [!info]+ IEEE 802
> L'IEEE 802 è un insieme di standard per la tecnologia delle [[LAN|reti locali (LAN)]] e delle [[MAN|reti metropolitane (MAN)]].
> 
> Lo standard IEEE 802 definisce diversi tipi di tecnologie di rete e e specifica i protocolli di comunicazione tra i dispositivi che utilizzano queste tecnologie. 
> Ha creato molti standard di successo, come ad esempio lo standard *Ethernet*.
> 
> Lo standard IEEE 802 divide il [[008 Livello Data Link|data link]] in due sottolivelli: **[[LLC]]** e **[[MAC (Indirizzo e Sottolivello)#Sottolivello MAC|MAC (sottolivello)]]**.
> 
> ![[Pasted image 20230425144043.png|450]]
^34aead

---
> L'**Ethernet** è una tecnologia di rete [[LAN]] che consente a diversi dispositivi di *comunicare* tra di loro all'interno di una *stessa rete locale*. È stato uno dei primi standard di rete elettronica ad essere sviluppato ed è diventato uno dei protocolli di rete più diffusi al mondo.

![[Pasted image 20230425145327.png]]

Ethernet utilizza un [[011 Collegamenti di rete nel Data Link|metodo di accesso al mezzo di trasmissione]] denominato [[013 Protocolli ad accesso casuale#CSMA/CD|CSMA/CD]].

### Pacchetti Ethernet
I **Pacchetti Ethernet** sono pacchetti di dati che vengono trasmessi attraverso una rete Ethernet. Questi pacchetti di dati contengono *informazioni necessarie per instradare i dati* attraverso la rete e per garantire la consegna al destinatario corretto.

I pacchetti Ethernet sono composti da: 
- Un <font color="#00C575">header</font>.
	- *Preambolo*: una sequenza di bit utilizzata per sincronizzare la ricezione del pacchetto;
	- Indirizzo MAC di *destinazione*;
	- Indirizzo MAC di *origine*;
	- *Tipo di protocollo* (ethertype): indica il tipo di protocollo utilizzato per il carico utile del pacchetto, ad esempio IPv4, IPv6, ARP, ecc.
- Un <font color="#FF6611">payload</font> (data): contiene i dati effettivi da trasmettere attraverso la rete;
- [[009 Rilevazione Errori nel Data Link#Cyclic Redundancy Code (CRC)|CRC]]: per rilevare errori nel pacchetto.

![[Pasted image 20230425151012.png]]


Un frame è valido solamente se è lungo almeno *64 byte*.

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
- https://youtu.be/CZmGuPjJfQI