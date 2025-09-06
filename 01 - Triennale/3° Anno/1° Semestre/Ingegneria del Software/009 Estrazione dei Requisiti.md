---
aliases:
  - Requirements Elicitation
  - Tracciabilità
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Estrazione
cssclasses:
  - 
---
>L'**Estrazione dei Requisiti** (*Requirements Elicitation*) si concentra sulla *descrizione dello scopo* del sistema attraverso la raccolta dei [[008 Requisiti|Requisiti]]. Si tratta di una comunicazione tra sviluppatori, clienti e utenti per definire un nuovo sistema.

Richiede la collaborazione tra diverse persone con specializzazioni diverse, dove:
- gli utenti e i clienti si specializzano sul [[002 Sistemi, Modelli e Viste#^ae7aa9|Dominio di Applicazione]];
- gli sviluppatori si specializzano sul [[002 Sistemi, Modelli e Viste#^0f8d39|Dominio di Soluzione]].

La mancata comunicazione e comprensione reciproca dei rispettivi ambiti porta a un sistema difficile da utilizzare o che semplicemente non riesce a supportare il lavoro dell'utente.

> [!info] Input ed output della fase
> Questa attività comincia da un [[010 Problem Statement|Problem Statement]] e produce una [[012 Specifica dei Requisiti|Specifica dei Requisiti]].


![[Pasted image 20240121111342.png|800]]



> [!question] Cosa fa parte dei requisiti?
>*Fanno parte dei requisiti* le funzionalità del sistema, l'interazione tra l'utente e il sistema, gli errori che il sistema può rilevare e gestire e le condizioni ambientali in cui il sistema funziona.
>
>Invece *non fanno parte dei requisiti* la struttura del sistema, la tecnologia di implementazione scelta per costruire il sistema, la progettazione del sistema, la metodologia di sviluppo e altri aspetti non direttamente visibili all'utente.

## Tipi di Estrazione dei Requisiti
Le attività di estrazione dei requisiti possono essere classificate in tre categorie:
- **Greenfield Engineering**: lo sviluppo *comincia da zero*, senza un sistema già esistente, dove i requisiti sono estratti dagli utenti e clienti; ^17dc3c
- **Re-engineering**: lo sviluppo riguarda il *re-design o re-implementazione* di un sistema già esistente attraverso nuove tecnologie; ^d7a017
- **Interface Engineering**: lo sviluppo riguarda la *creazione di nuovi servizi* per un sistema già esistente *in un nuovo ambiente*.

## Convalidazione dei Requisiti
I [[008 Requisiti|Requisiti]] vengono continuamente *convalidati* con il cliente e l'utente.

>La **Convalida** è una fase critica nel processo di sviluppo, poiché sia il cliente che lo sviluppatore *dipendono dalla Specifica dei Requisiti*. 

La convalida dei requisiti comporta il controllo che *la specifica dei requisiti prodotta sia*:
- **Completa**: se sono descritti tutti gli scenari possibili nel sistema, compresi i comportamenti eccezionali;
- **Coerente** (*consistency*): se i requisiti funzionali o non funzionali non si contraddicono tra di loro;
- **Corretta**: se rappresenta accuratamente il sistema di cui il cliente ha bisogno e che gli sviluppatori intendono costruire.
- **Realizzabile**: se i requisiti possono essere implementati e consegnati in maniera realistica;
- **Tracciabile**: se ogni funzione del sistema può essere ricondotta a un insieme corrispondente di requisiti funzionali.

> [!error] Problema
> I requisiti cambiano molto rapidamente durante la fase di estrazione dei requisiti.
> Una soluzione sarebbe quella di mettere i requisiti in uno spazio condiviso tra i clienti e gli sviluppatori. 

## Gestire l'Estrazione dei Requisiti
Gli sviluppatori devono trovare un modo per arrivare ad una soluzione finale con il cliente.
Ci sono diverse tattiche.
### Joint Application Design
La **Joint Application Design (JAD)** è una *singola sessione* in cui gli utenti, sviluppatori e clienti si riuniscono per presentare i loro punti di vista, ascoltare altri punti di vista e *negoziare* per poi arrivare ad una soluzione accettabile da tutte le parti.

L'output è il *Documento JAD*, una specifica dei requisiti completa che include tutte le decisioni prese durante la sessione.

### Tracciabilità
La **Tracciabilità** è l'abilità di seguire l'intero ciclo di vita di un requisito. Includendo la *fonte* del requisito (chi lo ha creato) fino a *quali aspetti del sistema affligge*.

È utilizzata per avere una visione chiara del progetto e rendere meno complessa e lunga un'eventuale fase di modifica di un aspetto del sistema.



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