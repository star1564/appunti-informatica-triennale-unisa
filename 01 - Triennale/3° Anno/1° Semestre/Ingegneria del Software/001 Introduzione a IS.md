---
aliases:
  - Software
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Introduzione
cssclasses:
  - 
---
## Software
>Un **Software** è un *insieme di codice, dati e documentazione correlata* che viene *progettato, sviluppato e mantenuto* per svolgere determinate funzioni o compiti su un sistema informatico.

Quindi il software è un prodotto:
- *Invisibile*: il software non può essere fisicamente toccato o visto;
- *Intangibile*: l'esistenza del software si manifesta solo attraverso l'interazione con un dispositivo e la visualizzazione di output;
- *Malleabile*: il software può essere facilmente modificato o adattato alle esigenze specifiche dell'utente;
- *Facilmente duplicabile*;
- *Molto costoso*.

I prodotti software possono essere:
- **Generici**: sono dei software prodotti da aziende e utilizzati da un ampio bacino di utenza diversificato;
- **Specifici**: sono dei software commissionati ed utilizzati da uno specifico cliente.

Il prezzo dei prodotti specifici può essere inferiore rispetto a quello dei prodotti generici, tuttavia richiedono anche l'impegno maggiore.

### Programma e Prodotto Software
| Programma | Prodotto Software |
| ---- | ---- |
| Un programma è *un insieme di istruzioni* fornite a un computer per compiere una specifica attività | Il software è *un prodotto industriale* che contiene tutti gli "artefatti" della progettazione |
| Poco costoso | Il costo è circa 10 volte il costo del programma |
| L'autore è anche l'utente | È usato da persone diverse da chi lo ha sviluppato |
| Non è quasi mai documentato | Ci sono diverse documentazioni prodotte |
| Approccio *informale* allo sviluppo | Approccio *formale* allo sviluppo |

### I problemi della produzione del Software
I problemi che si possono incontrare nella produzione del software dipendono da vari fattori: 
- Costi;
- Ritardi;
- Abbandoni;
- Inaffidabilità e Malfunzionamenti.

## Ingegneria del Software
>L'**Ingegneria del Software** riguarda la costruzione di software *di grandi dimensioni, di notevole complessità e sviluppati in gruppo*. Tutto questo attraverso un approccio sistematico e dettato da:
>- Principi;
>- Metodi e Metodologie; 
>- Tecniche;
>- Strumenti.

![[Terminologie Generali IS]]

### Principi e dell'IS
I **Principi** riguardano:
- *Rigore* e la *formalità*;
- *Separazione di aspetti diversi* dell'attività;
- *Modularità*: suddividere un sistema complesso in parti più semplici;
- *[[Astrazione sui Dati|Astrazione]]*: si individuano gli aspetti fondamentali e quelli da ignorare;
- *Anticipazione del cambiamento*: per favorire l'evoluzione del software;
- *Generalità*: tentare di creare un sistema in modo più generale possibile;
- *Incrementalità*: lavorare attraverso l'iterazione di step successivi.

### Attività nell'IS
Ci sono tre principali attività inerenti all'Ingegneria del Software:
- L'ingegneria del software è un'attività di **modellazione**: gli ingegneri del software *affrontano la complessità* attraverso la modellazione, concentrandosi in ogni momento solo sui dettagli rilevanti e ignorando tutto il resto;
- L'ingegneria del software è un'attività di **risoluzione dei problemi** (*problem-solving*): i modelli vengono utilizzati per cercare una *soluzione accettabile* guidata dalla sperimentazione. Gli ingegneri del software *non dispongono di risorse infinite* e sono vincolati dal budget e dalle scadenze;
- L'ingegneria del software è un'attività di **acquisizione dei requisiti**: nel modellare il dominio dell'applicazione e della soluzione, gli ingegneri del software *raccolgono i dati relativi ai requisiti*, li organizzano in informazioni e li formalizzano all'interno di *documentazioni*.


___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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
- [GeeksForGeeks](https://www.geeksforgeeks.org/software-engineering)