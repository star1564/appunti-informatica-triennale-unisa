---
aliases:
  - RAD
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Analisi dei Requisiti - Analisi
cssclasses:
  - 
---
>Il **Requirement Analysis Document (RAD)** contiene il risultato dell'[[009 Estrazione dei Requisiti|Estrazione dei Requisiti]] e dell'[[013 Analisi dei Requisiti|Analisi dei Requisiti]]. È il documento più importante del progetto, perché serve sia da *contratto tra gli sviluppatori ed il cliente* sia da un punto di partenza per la *comunicazione tra gli sviluppatori*.

Il RAD descrive completamente il sistema in termini di [[008 Requisiti#Requisiti Funzionali|Requisiti Funzionali]] e [[008 Requisiti#Requisiti Non Funzionali|Requisiti Non Funzionali]].

1. La prima parte del RAD, **Introduzione**, ha lo scopo di *fornire una breve panoramica* dell'intero progetto:
   - *Funzioni* del sistema;
   - *Motivi per lo sviluppo* del sistema; 
   - *Ambiti* (scope) del sistema; 
   - *Riferimenti al contesto di sviluppo* (come il [[010 Problem Statement|problem statement]] scritto dal cliente, riferimenti a sistemi esistenti, studi di fattibilità);
   - *Obiettivi e i criteri di successo* del progetto.
2. La seconda parte del RAD, **Sistema Corrente**, descrive le *caratteristiche del sistema attuale* (già esistente). 
   - Se non esiste già un sistema, questa sezione si salta oppure si descrive un sistema avversario.
3. La terza parte del RAD, **Sistema Proposto**, documenta l'estrazione dei requisiti e l'analisi dei requisiti del nuovo sistema. Si divide in quattro parti:
	- *Panoramica*: una panoramica generale del funzionamento del sistema;
	- *Requisiti Funzionali*: descrive le funzionalità principali (ad alto livello) del sistema;
	- *Requisiti Non Funzionali*: descrive i requisiti che non sono relazionati alle funzionalità ad alto livello;
	- *Modelli del Sistema*:
		- [[011 Scenari e Casi d_Uso#Scenario|Scenari]]; 
		- [[011 Scenari e Casi d_Uso#Caso d'Uso|Casi d'Uso]]; 
		- [[014 Modello ad Oggetti|Modello ad Oggetti]] (<font color="#FF6611">ottenuto dall'analisi dei requisiti</font>): documenta in dettaglio tutti gli oggetti identificati, i loro attributi e, quando si utilizzano i diagrammi di sequenza, le operazioni. Mentre ogni oggetto è descritto con definizioni testuali, le relazioni tra gli oggetti sono illustrate con diagrammi di classe;
		- [[016 Modello Dinamico|Modello Dinamico]] (<font color="#FF6611">ottenuto dall'analisi dei requisiti</font>): documenta il comportamento del modello di oggetti in termini di diagrammi di macchine a stati e diagrammi di sequenza. Sebbene queste informazioni siano ridondanti rispetto al modello dei casi d'uso, i modelli dinamici ci permettono di rappresentare con maggiore precisione comportamenti complessi, compresi i casi d'uso che coinvolgono molti attori;
		- [[004 Modelli di CVS#^920367|Mock-Up]] che mostrano l'UI ed i percorsi.
4. Un **[[012 Specifica dei Requisiti#5 - Creare un Glossario di Termini (oggetti partecipanti)|Glossario dei Termini]]**.

> [!question]+ Il Requirement Analysis Document risponde alle seguenti domande
> 1. *Quali sono le trasformazioni?* → Modellazione funzionale: creare scenari e casi d'uso parlando con il cliente, osservando e facendo esperimenti.
> 2. *Qual è la struttura del sistema?* → Modellazione ad oggetti: creare i Class Diagrams, identificare classi, associazioni e molteplicità.
> 3. *Qual è il comportamento del sistema?* → Modellazione dinamica: creare i sequence diagrams, mostrare la sequenza degli eventi scambiati tra gli oggetti, identifiare le dipendenze e concorrenza tra eventi e creare diagrammi di stato.


---

> [!tip] <font color="orange">Template</font>
>```
>1. Introduzione
>	1.1 Finalità del Sistema
>	1.2 Ambiti del Sistema
>	1.3 Criteri di successo ed Obbiettivi per il Progetto
>	1.4 Definizioni, acronimi ed abbreviazioni
>	1.5 Riferimenti
>2. Sistema Attuale
>3. Sistema Proposto
>	3.1 Panoramica
>	3.2 Requisiti Funzionali
>	3.3 Requisiti Non Funzionali
>		3.3.1 Usabilità
>		3.3.2 Affidabilità
>		3.3.3 Prestazioni
>		3.3.4 Supportabilità
>		3.3.5 Implementazione
>		3.3.6 Interfaccia
>		3.3.7 Packaging
>		3.3.8 Legal
>	3.4 Modelli del Sistema
>		3.4.1 Scenari
>		3.4.2 Modello dei Casi d'Uso
>		3.4.3 Object Model (ottenuto dall'analisi dei requisiti)
>			3.4.3.1 Dizionario dei Dati
>			3.4.3.2 Diagrammi delle Classi
>		3.4.4 Dynamic Model (ottenuto dall'analisi dei requisiti)
>		3.4.5 User Interface e Percorsi
>4. Glossario dei Termini
>```


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