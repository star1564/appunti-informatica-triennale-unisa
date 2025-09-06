---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Introduzione
cssclasses:
  - 
---
Una **Base di Dati** (*Database*) è una *collezione di dati correlati* strutturati ed archiviati in un sistema informatico.  ^f0d93b

Di solito i dati vengono organizzati sotto forma di *tabelle* per rendere tutto più efficiente. Una base di dati può essere di qualsiasi dimensione e di varia complessità.

Per **Dati** si intendono fatti noti che possono essere memorizzati e che hanno un significato implicito.

Per esempio, una lista di studenti è un database. 

Ogni base di dati ha le seguenti proprietà:
- rappresenta un certo aspetto del mondo reale, talvolta detto il [[Mini-Mondo|mini-mondo o universo del discorso]];
- è una collezione di dati logicamente coerenti con un significato (non si può costruire un database con dati non coerenti);
- è progettata, costruita e popolata con dati per uno scopo specifico.

## GESTIRE UN DATABASE
Un database può essere gestito manualmente (tramite archivi cartacei) oppure computerizzata attraverso un [[DBMS|Sistema di Gestione di Basi di Dati (DBMS)]]. Le funzionalità di questo sistema sono:

- **Costruire** una base di dati significa *immagazzinare i dati* in un mezzo di memorizzazione controllato dal DBMS;
- La **Manipolazione**, che include funzioni per reperire dati specifici, l'aggiornamento dei dati e la generazione di prospetti (report);
- **Condividere** un database consente a più utenti e programmi di accedere direttamente ai dati;
- Gestire i **[[Metadato|meta-data]]**, memorizzati dal DBMS nel *catalogo (o dizionario) di sistema*;
- **Protezione** del database, che comprende la protezione di sistema dai guasti sia la protezione da accessi non autorizzati per scopi di sicurezza.

Un [[Programma Applicativo]] accede alla base di dati inviando delle [[Query|interrogazioni (query)]] o richieste al DBMS. 

![[Pasted image 20220928160338.png|700]]

---
### FILE PROCESSING E DATABASE
Non è necessario usare software DBMS a scopi generici per realizzare una base di dati computerizzata. 
È possibile scrivere un proprio insieme di programmi per produrre e mantenere la base di dati, creando un software DBMS *personale* con *scopi specifici*. 

In questo tipo di sistema, chiamato **File Processing**, ogni utente definisce ed implementa i file necessari per una specifica applicazione, con notevole *spreco di risorse e ridondanza dei dati*. 

La struttura di un file dati è immersa nei programmi che accedono al file, quindi un cambiamento nella struttura del file richiede un cambiamento in tutti i programmi.

Invece in un **database** si definisce una volta per tutte un *singolo repository* di dati, poi usato dai vari utenti.

![[Pasted image 20220928161707.png|700]]

___
[[000 Indice BD|↖ Ritorna all'indice ↖]]

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

