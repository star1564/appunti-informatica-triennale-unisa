---
aliases:
tags:
  - corsi/informatica/sicurezza
paragrafo: Introduzione Sicurezza
cssclasses:
  - 
---
>La **Sicurezza sui Dati** (*Cybersecurity*) è la *protezione delle informazioni* memorizzate, trasmesse ed elaborate in una [[001 Introduzione a RC|rete di calcolatori]], compreso l'Internet. 
>I metodi di protezione comprendono politiche e procedure organizzative e mezzi tecnici come la [[003 Crittografia|crittografia]] e i [[028 SSL e TLS|protocolli di comunicazione sicuri]].

La sicurezza sui dati si basa su:
- **Confidenzialità** - Assicura che le informazioni private o riservate *non siano rese disponibili* o divulgate *a persone non autorizzate*. Garantisce anche la gestione della privacy dei dati. ^ff3019
- **Integrità** - Assicura che i dati (sia memorizzati che nei [[008 Livello Data Link|pacchetti]] trasmessi) possano essere *modificati solo da chi è autorizzato* (scrittura, cambiamenti, cancellazione, ecc...). ^9a165d
- **Autenticazione** - È la proprietà che un oggetto digitale ha quando è *autentico* e può essere *verificato e fidato*. La fiducia si trova nella validità di una trasmissione, di un messaggio o dell'originatore di un messaggio. Ciò significa verificare che gli utenti *siano chi dicono di essere* e che ogni input che arriva al sistema provenga da una *fonte affidabile*. ^61b340
- **Non Ripudio** - La garanzia che al mittente delle informazioni venga fornita la *prova della consegna* e al destinatario *la prova dell'identità* del mittente, in modo che nessuno dei due possa in seguito negare di aver elaborato le informazioni (come la pec).
- **Disponibilità Risorse** - Assicura che i sistemi funzionino normalmente e che il servizio *non venga negato agli utenti autorizzati*.
- **Controllo degli Accessi** - Assicura la *limitazione ed il controllo dell'accesso* alle informazioni da o per il sistema.
- **Anonimia** - Assicura la *protezione* dell'identità o del servizio utilizzato.

___
[[000 Indice Sicurezza|↖ Ritorna all'indice ↖]]

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