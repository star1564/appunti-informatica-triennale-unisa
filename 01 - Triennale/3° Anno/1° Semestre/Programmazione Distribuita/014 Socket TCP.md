---
aliases: Socket
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Socket
cssclasses:
  - 
---
## Socket
> I **Socket [[030 TCP|TCP]]** sono gli endpoint di una comunicazione *bidirezionale* sulla rete che unisce due programmi attraverso la rete o sulla stessa macchina. Ad ogni socket viene assegnato un numero di [[032 Porte|Porta]] che identifica l'applicazione incaricata di dover trattare i dati.

Questa astrazione permette di ricevere e trasmettere dati attraverso un semplice *stream di dati*.

Normalmente si identificano i due computer coinvolti in un socket con il nome di [[Client]] e [[Server]].

![[Pasted image 20230908172042.png|700]]

Il server è in *esecuzione* ed attende che qualche client *richieda la connessione*. Il procedimento di connessione prevede che il server debba accettare la connessione e che **assegni un nuovo socket** per la comunicazione. In questo modo, il server può tornare ad accettare connessioni da altri client.

### Socket TCP
Una socket nel protocollo [[030 TCP|TCP]] è identificata da 4 parametri:
- [[Indirizzo IP|Indirizzi IP]] di *origine e destinazione*;
- Numeri di porta di *origine e destinazione*.

### Socket UDP
Una socket nel protocollo [[031 UDP|UDP]] è identificata da 2 parametri:
- [[Indirizzo IP]] di *destinazione*;
- Numero di porta di *destinazione*.

## java.net
Il pacchetto `java.net` offre due classi per i Socket, usate per *gestire la fase di accettazione* da parte del server e la *comunicazione bidirezionale* tra il client e il server. Questi sono `ServerSocket` e `Socket`.

![[Immagine 2023-10-06 162238.png]]

Quando la connessione viene chiusa il `data socket` affetto viene eliminato.

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