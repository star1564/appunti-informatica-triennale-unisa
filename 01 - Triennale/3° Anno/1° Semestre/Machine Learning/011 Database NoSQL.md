---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Data Engineering
cssclasses:
  - 
---
>**NoSQL** (*Not Only SQL*) è il termine che raggruppa un insieme di tecnologie per la persistenza dei dati che si differenziano dai database relazionali, spesso rinunciando ad alcune caratteristiche dei RDBMS. Queste soluzioni trovano impiego in scenari che coinvolgono *volumi massicci di dati* e *query veloci*.


> [!failure] Gli svantaggi dei Database RELAZIONALI
>Per alcune esigenze specifiche, il [[010 Modello Relazionale|modello relazionale]] può risultare *restrittivo* per diverse ragioni:
>- Richiede rigidamente che i *dati rispettino uno schema* predeterminato;
>- La gestione degli schemi può risultare complessa e onerosa;
>- L'esecuzione di query [[023 Definizioni SQL|SQL]] per applicazioni specializzate può rivelarsi problematica.

## Caratteristiche
I database NoSQL presentano comunemente le seguenti proprietà:
- **Non relazionali (schemaless)**: consentono di memorizzare dati dinamici senza uno schema rigido, permettendo l'inclusione di attributi "on the fly" anche senza definizione preventiva;
- **Distribuiti**: offrono flessibilità nella clusterizzazione e replicazione dei dati, consentendo di distribuire lo storage su più nodi per realizzare sistemi altamente fault-tolerant;
- **Scalabili orizzontalmente**: contrariamente alla scalabilità verticale, consentono una scalabilità su larga scala, gestendo grandi quantità di informazioni;
- **Open Source**: molte di queste soluzioni sono basate su licenze open source, promuovendo la collaborazione e la trasparenza nella comunità.

### ACID in NoSQL
Per quanto riguarda le proprietà [[ACID]], alcuni database NoSQL *garantiscono solo alcune di esse* per migliorare le prestazioni:
- *La consistenza potrebbe non essere sempre garantita*, dando vita al concetto di "eventual consistency," con una certa latenza prima che le modifiche al database diventino visibili.
- *La durabilità potrebbe non essere garantita in ogni situazione*, specialmente in ambienti distribuiti, dove il malfunzionamento di un nodo può compromettere la sincronizzazione dell'intera rete.

### Vantaggi dei Database NoSQL
I Database NoSQL portano diversi vantaggi:
- **Grossi Volumi di Dati**: i Database NoSQL eccellono nella gestione di ingenti quantità di dati, garantendo efficienza anche con carichi di lavoro massicci.

- **Minore Manutenzione**: la struttura flessibile e la capacità di adattarsi dinamicamente ai cambiamenti consentono una gestione semplificata e meno onerosa rispetto ai database tradizionali.

- **Velocità di Risposta alle Query**: la natura distribuita e scalabile dei database NoSQL li rende ideali per fornire risposte veloci e efficienti alle query, ottimizzando le prestazioni in ambienti con elevati carichi di lavoro.

- **Transazioni BASE (Abbandono delle Proprietà ACID)**:
	- *Garanzia di Disponibilità*: i sistemi NoSQL prioritizzano la disponibilità dei dati anche in presenza di molteplici guasti, mantenendo il servizio operativo.
	- *Consistenza Basata sul Futuro*: si allontanano (quasi completamente) dal requisito di consistenza tipico dei modelli ACID, concentrando l'attenzione su una convergenza futura verso uno stato consistente.

- **CAP Theorem**: i database NoSQL affrontano le sfide del CAP Theorem, bilanciando la coesistenza tra coerenza, disponibilità e tolleranza al partizionamento in ambienti distribuiti.

### Tipologie di DB NoSQL
![[Pasted image 20231215155124.png|400]]

### Quando conviene
È utile scegliere un Database NoSQL quando:
- La struttura dei dati *non è definibile a priori*;
- I dati disposti nei vari oggetti *sono molto collegati tra loro* e il [[018 Operazioni di Join|JOIN]] rischia di non essere lo strumento ottimale;
- Sarebbe da prediligere la navigazione tra oggetti sfruttando i riferimenti tra i vari nodi di informazione;
- Necessitiamo di *prestazioni più elevate*, potendo considerare troppo stringenti le proprietà ACID per il nostro campo di applicazione.

Però, in moltissimi casi, il modello relazionale rimane l'opzione migliore, dato che la struttura rigida non è necessariamente un problema.

___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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