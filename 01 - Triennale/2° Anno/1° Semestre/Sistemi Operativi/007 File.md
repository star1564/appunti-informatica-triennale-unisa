---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: File System
cssclasses:
  - 
---
Il sistema operativo fornisce un'[[001 Introduzione a OOP#^1dfc00|astrazione]] delle caratteristiche fisiche dei propri dispositivi di memorizzazione, definendo un'unità di memorizzazione logica, il **File**.

Un file è un *insieme di informazioni correlate*, conservate sulla memoria secondaria (non volatile) a cui è stato assegnato un nome. Questi contengono dati (testo) o programmi (codice binario).

#### STRUTTURA DI UN FILE
Un file ha una **struttura** ([[1003.L-SO Stat|stat]]) definita secondo il tipo:
- un *file di testo* è una sequenza di caratteri organizzati in righe e pagine;
- un *file sorgente* è una sequenza di istruzioni, subroutine e funzioni formattati secondo il linguaggio scelto;
- un *file oggetto* è una sequenza di byte organizzati in blocchi che risultano comprensibili al linker del SO;
- un *file eseguibile* consiste di una serie di sezioni di codice che il il caricatore può portare in memoria ed eseguire.

Un file ha vicino al nome una *estensione*, che specifica che tipo di file si ha (.jpg, .pdf, .c, ...). 

> [!note]- Estensioni in UNIX
> In alcuni sistemi operativi (come UNIX) l'estensione non è necessaria, dato che memorizzano un *codice* all'inizio di alcuni tipi di file in modo da indicarne in modo generico il tipo (eseguibile, script shell, ...).

#### ATTRIBUTI DI UN FILE
A ciascun file vengono associati degli **Attributi**, che possono facilitare l'uso e le possibili operazioni che si possono eseguire.

Le informazioni sui file sono mantenute nella struttura delle directory, che risiede sul disco.

- *Nome*: il nome simbolico del file è l'unica informazione in forma umanamente leggibile;
- *Identificatore*: è un etichetta unica (spesso numerica) che identifica il file all'interno del file system;
- *Tipo*: è necessario solo ai sistemi che gestiscono differenti tipi di file (Normali, [[010 Directory|directory]], link, ...);
- *Posizione Fisica*: è un puntatore alla locazione fisica del file nel dispositivo;
- *Posizione Logica*: è il pathname del file (non viene spesso memorizzata);
- *Dimensione*: si tratta della dimensione corrente del file (in byte);
- *[[Permessi File|Permessi]]*: determina chi può leggere, scrivere ed eseguire il file;
- *Ora, data di modifica e identificativo dell'utente*: queste informazioni possono essere utili per la protezione, sicurezza ed il monitoraggio d'uso.

___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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
