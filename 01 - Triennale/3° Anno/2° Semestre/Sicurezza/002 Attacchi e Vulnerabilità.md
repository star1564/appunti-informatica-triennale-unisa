---
aliases: 
  - Denial of Service
  - DDos
  - Dos
  - Weakest link principle
tags:
  - corsi/informatica/sicurezza
paragrafo: Introduzione Sicurezza
cssclasses:
  - 
---
## Vulnerabilità e Attacco
In sicurezza si utilizzano i seguenti termini:
- **Vulnerabilità** - Una debolezza (qualsiasi circostanza o evento) con il *potenziale* di avere *un impatto negativo sulle operazioni* dell'organizzazione.
	- *Weakest link principle* - Un sistema è forte solo quanto il suo componente o aspetto più debole.
- **Attacco** - Qualsiasi tipo di attività dannosa che *sfrutta vulnerabilità di un sistema* per tentare di raccogliere, interrompere, negare, degradare o distruggere *le risorse del sistema* informativo o le informazioni stesse.

La **difesa** deve quindi adattarsi *dinamicamente* ai nuovi tipi di attacchi per avere maggiori possibilità di resistere.

## Tipi di Attacchi

### Attacchi Passivi
>Gli **Attacchi Passivi** consistono nell'*intercettazione o nel monitoraggio delle trasmissioni*. L'obiettivo dell'attaccante è ottenere le informazioni trasmesse. 

Esistono due tipi di attacchi passivi: 
- la *divulgazione del contenuto* dei messaggi
- l'*analisi* del traffico

![[Pasted image 20240313130842.png|450]]

Gli attacchi passivi sono molto difficili da rilevare perché *non comportano alcuna alterazione dei dati*. In genere, il traffico di messaggi viene inviato e ricevuto in modo apparentemente normale e né il mittente né il destinatario sono a conoscenza del fatto che una terza parte abbia letto i messaggi o osservato lo schema del traffico.

Tuttavia, è possibile impedire il successo di questi attacchi, di solito attraverso la [[003 Crittografia|crittografia]]. Pertanto, l'enfasi nella gestione degli attacchi passivi è sulla prevenzione piuttosto che sulla rilevazione.

### Attacchi Attivi
>Gli **Attacchi Attivi** comportano una *modifica del flusso di dati* o la creazione di un *flusso falso*. 

![[Pasted image 20240313131033.png|660]]

Possono essere suddivisi in quattro categorie: 
- Il *replay* comporta la cattura passiva di un'unità di dati e la sua successiva ritrasmissione per produrre un effetto non autorizzato. ^159950
- Il *masquerade* avviene quando un'entità finge di essere un'altra entità.
- La *modifica dei dati* significa che una parte di un messaggio legittimo viene alterata o che i messaggi vengono ritardati o riordinati per produrre un effetto non autorizzato.
- Il **denial of service** impedisce o ostacola il normale utilizzo o la gestione delle strutture di comunicazione. ^denialOfService
	- Si ha un *DoS* quando l'attacco coinvolge un solo sistema attaccante e un solo sistema attaccato.
	- Si ha un *DDos* quando l'attacco coinvolge diversi sistemi che attaccano un singolo sistema. ^7a6bd8


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