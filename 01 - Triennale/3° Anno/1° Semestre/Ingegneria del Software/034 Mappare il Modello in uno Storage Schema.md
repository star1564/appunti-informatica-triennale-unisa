---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Implementazione
cssclasses:
  - 
---
È necessario mappare gli **oggetti persistenti** in strutture dati che possono essere memorizzate nei sistemi di gestione dei dati selezionati durante il [[019 System Design|System Design]].

Se si usano dei **[[035 Object Relational Mapping|database object-oriented]]** non devono essere effettuate trasformazioni.

> [!note] Dove scrivere il risultato
> Il risultato sarà un modello dei dati persistenti che può essere inserito nell'[[028 Object Design Document|ODD]] oppure in un nuovo file chiamato **Database Design Document (DDD)**.

## Mappare Classi ed Attributi
- Le **Classi** vengono mappate in *Tabelle con lo stesso nome*;
- Gli **Attributi** vengono mappati in una *colonna della Tabella* con lo stesso nome dell'attributo;
- Ogni *riga* della tabella corrisponde ad un'**Istanza** della classe.
- Si sceglie un attributo come **[[ER5_ Attributi Chiave|Chiave Primaria]]**.

![[Pasted image 20240125130748.png|600]]

Mantenendo gli stessi nomi nel [[028 Object Design Document|Modello di Object Design]] e nelle Tabelle, si garantisce la [[009 Estrazione dei Requisiti#Tracciabilità|Tracciabilità]] tra le due rappresentazioni.

## Mappare Associazioni
Per quanto riguarda le [[UML 5 - Diagramma di Classi#^a78046|associazioni uno a uno]] o [[UML 5 - Diagramma di Classi#^65d403|uno a molti]] è necessario utilizzare una [[011 Vincoli del Modello Relazionale#VINCOLI DI INTEGRITÀ REFERENZIALE|Chiave Esterna]] che collega le due tabelle.

Se l'associazione è uno a molti, la chiave esterna è *nella classe del lato "molti"*.

![[Pasted image 20240125131336.png|700]]


Se l'associazione è uno a uno, la chiave esterna *può essere in qualsiasi delle due tabelle*.

Le [[UML 5 - Diagramma di Classi#^7f05ca|associazioni molti a molti]], sono implementate usando una tabella aggiuntiva che ha le due colonne contenenti le chiavi esterne delle tabelle relative alle classi in relazione. Questa tabella è chiamata **tabella associativa**.

![[Pasted image 20240125131412.png|700]]


## Mappare l'ereditarietà
I [[010 Modello Relazionale|database relazionali]] non supportano l'[[015 Ereditarietà|Ereditarietà]]. Esistono due modi per mapparla in uno schema.

Il compromesso tra i due è la *modificabilità e tempo di risposta*.
In generale, è necessario esaminare la probabilità di modifiche rispetto ai requisiti di prestazioni nel contesto specifico dell'applicazione.

Per le gerarchie di ereditarietà profonde, questo può rappresentare una differenza significativa in termini di prestazioni.
### Mapping Verticale
Quando c'è una gerarchia tra classi, *si creano tabelle separate* per la **classe principale** e le sue **sottoclassi**. 

La tabella della classe principale ha colonne per *tutti i suoi attributi* ed aggiunge una *colonna extra per indicare la sottoclasse* associata a ciascun dato. 
Le tabelle delle sottoclassi riflettono gli attributi della classe principale ma non appaiono. 

Tutte le tabelle condividono la *stessa chiave primaria*, che identifica univocamente gli oggetti. I dati nelle tabelle della classe principale e delle sottoclassi con lo stesso valore di chiave primaria si riferiscono allo stesso oggetto.

![[Pasted image 20240125135104.png|800]]


| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Si possono aggiungere attributi alla superclasse in modo semplice | Ricercare tutti gli attributi di un oggetto richiede una operazione di [[018 Operazioni di Join\|Join]] |
| Si possono aggiungere altre sottoclassi ed attributi alle in modo semplice |  |
| *Alta Modificabilità* | *Bassa Velocità* |


### Mapping Orizzontale
Un altro modo di realizzare l'ereditarietà è quello di *spingere gli attributi della superclasse nelle sottoclassi*, eliminando di fatto la necessità di una tabella di superclasse. In questo caso, ogni tabella di sottoclasse duplica le colonne della superclasse.

![[Pasted image 20240125135529.png|800]]


| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Gli oggetti non sono frammentati tra più tabelle | La modifica dello schema del database è più complessa e soggetta a errori |
| Le query sono più veloci |  |
| *Alta Velocità* | *Bassa Modificabilità* |





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
- [[013 Mapping DB Relazionale]]