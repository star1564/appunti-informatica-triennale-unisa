---
aliases:
  - CVS
  - Ciclo di Vita del Software
  - Ciclo di Sviluppo del Software
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Ciclo di Vita del Software
cssclasses:
  - 
---
> Il **Ciclo di Vita del Software (CVS)** rappresenta l'*intera evoluzione* di un'applicazione software. È il periodo di tempo che inizia quando un prodotto software è concepito e che termina quando il prodotto non è più disponibile per l'uso.

![[Pasted image 20240117110926.png|1400]]

In genere include le seguenti fasi:
1. *Fase di Pianificazione*: definizione dello scopo e dell'ambito del software, inclusi i compiti da svolgere e la gestione del progetto;
2. *Fase di Analisi*: identificazione e registrazione dei requisiti precisi definiti dai clienti;
3. *Fase di Progettazione*: creazione di un framework adatto alla soluzione del problema;
4. *Fase di Implementazione*: conversione da progetto a codice tangibile;
5. *Fase di Testing*: individua i problemi dell'intero progetto;
6. *Fase di Deployment*: si fa eseguire il prodotto completato sull'hardware definito o fornito dai clienti;
7. *Fase di Funzionamento e Mantenimento*: miglioramento del prodotto ed assistenza per garantire la longevità del software;
8. *Fase di Ritiro*: fase opzionale e terminale del ciclo di vita del software, che rende il servizio offerto dal prodotto non più disponibile.

Queste fasi possono sovrapporsi o essere eseguite iterativamente.

## Vista ad alto livello delle fasi di un CVS
I CVS si suddividono molto spesso in tre fasi:
- **Definizione** (*cosa* sviluppare): determinazione dei requisiti, informazioni da elaborare, funzioni e prestazioni attese, comportamento del [[002 Sistemi, Modelli e Viste#Sistema|Sistema]], interfacce;
- **Sviluppo** (*come* sviluppare): definizione del progetto, dell'architettura software, della strutturazione dei dati e la scelta del linguaggio di programmazione;
- **Manutenzione** (*modifiche* e *miglioramenti*).

![[Pasted image 20240117110952.png|1400]]

### Ciclo di Sviluppo del Software
> Il **Ciclo di Sviluppo del Software**, invece, è il periodo di tempo che inizia con la decisione di sviluppare un prodotto software e termina con la consegna del prodotto.

L'insieme delle fasi di questo ciclo è un sottoinsieme delle fasi del CVS: parte dalla fase di Analisi e termina alla fase di Deployment.

![[Pasted image 20240117111036.png|1400]]

Il CVS e il ciclo di sviluppo di un software sono due concetti correlati ma distinti nell'ambito dell'ingegneria del software.

In questo corso ci si concentra su queste fasi.

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
- https://www.split.io/blog/software-development-life-cycle-phases/