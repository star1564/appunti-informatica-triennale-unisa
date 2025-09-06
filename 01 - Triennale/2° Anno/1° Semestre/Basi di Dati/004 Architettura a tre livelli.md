---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modelli di Dati
cssclasses:
  - 
---
L'obbiettivo dell'**Architettura a Tre Livelli** (three-schema) è quello di separare le applicazioni utente dalla base di dati fisica. In questa architettura possono essere definiti schemi ai seguenti tre livelli.

![[Immagine 2022-10-01 113221.png|700]]

####  1) LIVELLO INTERNO
Il **Livello Interno** ha uno *schema interno*, che descrive la struttura di memorizzazione fisica della base di dati. 

Lo schema interno usa un [[002 Modelli di Dati#MODELLI DI BASSO LIVELLO FISICI|modello di dati fisico]] e descrive i dettagli completi della memorizzazione dei dati e dei cammini di accesso per la base di dati.

> [!example]+ <font color="orange">Esempio</font>
> Prendendo in considerazione un database universitario, dividi i record del file studenti in tre partizioni sui dischi 5, 6 e 7.

#### 2) LIVELLO CONCETTUALE
Il **Livello Concettuale** ha uno *schema concettuale*, che descrive la struttura dell'intera base di dati per una comunità di utenti. 

Lo schema concettuale nasconde i dettagli delle strutture di memorizzazione fisica e si concentra sulla *descrizione* di [[ER1_Entità|Entità]], tipi di dati, [[ER6_Relazione tra Entità|Relazioni tra entità]], operazioni utente e vincoli.

Di solito si usa un [[002 Modelli di Dati#MODELLI AD ALTO LIVELLO CONCETTUALI|modello di dati ad alto livello]] oppure uno di [[002 Modelli di Dati#MODELLI RAPPRESENTAZIONALI LOGICI IMPLEMENTAZIONE|implementazione]].

> [!example]+ <font color="orange">Esempio</font>
> Il tipo `Studente` è un record composto da:
> - `nome`: è una stringa;
> - `matricola`: è una stringa;
> - `anno`: è un intero.

#### 3) LIVELLO ESTERNO
Il **Livello Esterno** o di **Vista** comprende un certo numero di *schemi esterni* o *viste utente* che definiscono un sottoinsieme del database.

Ciascuno schema esterno descrive la parte della base di dati a cui è interessato un particolare gruppo di utenti, nascondendo il resto.

Anche qui si usano modelli di dati ad alto livello.

> [!example]+ <font color="orange">Esempio</font>
> La segreteria didattica ha necessità di vedere tutte le informazioni sugli esami, ma non deve vedere informazioni sugli stipendi.

## VANTAGGI E SVANTAGGI
*Vantaggi*:
- L'architettura a tre livelli consente facilmente di ottenere una reale [[005 Indipendenza dei Dati|indipendenza dei dati]];
- Il database risulta più flessibile e scalabile.

*Svantaggi*:
- Il catalogo del [[DBMS]] multi-livello deve avere dimensioni maggiori, per includere informazioni su come trasformare le richieste e dati tra i vari livelli.
- Ci potrebbe essere delle inefficienze nel DBMS alla richiesta di una query.

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