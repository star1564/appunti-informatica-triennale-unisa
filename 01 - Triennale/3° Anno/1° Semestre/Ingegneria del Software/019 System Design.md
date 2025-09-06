---
aliases: 
  - Sottosistema
  - Subsystem
  - Coesione
  - Stratificazione
  - Partizionamento
  - Coupling
  - Accoppiamento
  - Architettura chiusa/aperta
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - System Design
cssclasses: 
---
>Il **System Design** costituisce *l'inizio della progettazione del sistema* e comprende la trasformazione di un [[013 Analisi dei Requisiti#Modello di Analisi|Modello di Analisi]] in un **Modello di System Design**. 
>Si focalizza sul [[002 Sistemi, Modelli e Viste#^0f8d39|Dominio di Soluzione]].

Si decideranno i processi, strutture dati, software ed hardware necessari per implementare il sistema.

L'obbiettivo principale del system design è quello di *decomporre il sistema in sottosistemi più piccoli*, per renderlo più semplice da gestire.

> [!info] Input ed output della fase
> Richiede il [[013 Analisi dei Requisiti#Modello di Analisi|Modello di Analisi]] come input e produce come output un Modello di System Design, specificando:
> - *Gli obbiettivi della progettazione*: le qualità del sistema da ottimizzare;
> - *[[020 Stili di Architettura|L'architettura del software]]*: il tipo di decomposizione in sotto-sistemi;
> - *I casi d'uso al boundary*: la configurazione del sistema, l'avvio, lo spegnimento e l'exception handling.

### Sottosistema
>Un **Sottosistema** è un *insieme di classi* che sono *in relazione tra di loro*. Aiutano a *ridurre la complessità* del dominio di soluzione.

Per modellare sottosistemi in UML, si possono usare i *Package* nel [[UML 5 - Diagramma di Classi|Class Diagram]].

![[Pasted image 20240119101959.png|600]]

>Un **Servizio** è un *insieme di operazioni correlate* che condividono un obbiettivo comune e che sono offerte dal sottosistema, per altri sottosistemi. 
>Questi *hanno origine dai casi d'uso dei sottosistemi*.

Tutti i servizi disponibili sono specificati all'interno dell'*Interfaccia del Sottosistema*.

![[Pasted image 20240118181109.png]]

## Coesione e Coupling
### Coesione
>La **Coesione** è il numero di [[024 Dipendenze e Tipi di Iniezioni#Dipendenza|Dipendenze]] *all'interno di un sottosistema*.

Se un sottosistema contiene molti oggetti che hanno una relazione tra di loro ed eseguono lavori simili, la **Coesione è Alta**.
Se invece ha un certo numero di oggetti senza relazioni tra di loro, la **Coesione è Bassa**.

### Coupling
>Il **Coupling** è il numero [[024 Dipendenze e Tipi di Iniezioni#Dipendenza|Dipendenze]] *tra due sottosistemi*.

- Se due sottosistemi sono [[024 Dipendenze e Tipi di Iniezioni#Loosely Coupled|Loosely Coupled]], allora questi sono relativamente indipendenti, quindi le modifiche al primo avranno poco impatto sul secondo e viceversa. Questo si chiama **Basso Coupling**. 
- Altrimenti si avrà un **Alto Coupling**.

## Ottenere un basso Coupling
### Stratificazione
>La **Stratificazione** (*Layering*) è un *raggruppamento gerarchico di sottosistemi*, dove ogni sottosistema in un certo livello *offre dei servizi ai livelli superiori* e riceve servizi dai livelli inferiori.

Il livello che non dipende da alcun servizio si chiama *livello inferiore*, invece il livello che non è usato da alcun sottosistema è chiamato *livello superiore*.

![[Pasted image 20240119101313.png|800]]

- In una **Architettura chiusa**, ogni livello può accedere solamente al *livello inferiore immediatamente al di sotto*. ^5952da
- In una **Architettura aperta**, un livello può accedere ad altri *livelli inferiori a qualsiasi profondità*. ^a75bda

### Partizionamento
>Una **Partizione** organizza i sottosistemi come *peer*, dove ogni peer fornisce differenti tipi di servizi agli altri sottosistemi.

Dividono verticalmente un sistema in svariati sottosistemi indipendenti.

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