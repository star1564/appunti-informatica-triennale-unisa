---
aliases:
tags:
  - corsi/informatica/machine_learning
paragrafo: Design di Sistemi ML
cssclasses:
  - 
---
>Formulare il tipo di problema è una fase molto importante. Esistono diverse tipologie di **task** di ML, identificati dal *tipo di output del modello*.

![[Pasted image 20231121112029.png|700]]

La distinzione più generale è tra problemi di: 
1. [[007 Task di Classificazione|Classificazione]]
	- [[007 Task di Classificazione#Classificazione Binaria|Classificazione Binaria]]
	- [[007 Task di Classificazione#Classificazione Multi-classe|Classificazione Multi-classe]]
		- [[007 Task di Classificazione#Alta e Bassa Cardinalità|Classificazione Multi-classe ad Alta cardinalità]]
		- [[007 Task di Classificazione#Alta e Bassa Cardinalità|Classificazione Multi-classe a Bassa cardinalità]]
	- [[007 Task di Classificazione#Multi-etichetta|Classificazione Multi-label]]
2. [[008 Task di Regressione|Regressione]]


## Da modello di regressione a classificazione e viceversa
Un modello di regressione può facilmente essere *convertito* in un modello di classificazione e viceversa. 

> [!example]- <font color="orange">Esempio</font>
>Ad esempio, la previsione del prezzo delle case può diventare un compito di classificazione se suddividiamo i prezzi delle case in fasce, come meno di $100,000, $100,000 - $200,000, $200,000 - $500,000, e così via, e prevediamo la fascia in cui dovrebbe trovarsi la casa.
>
>Il modello di classificazione delle email può diventare un modello di regressione se lo configuriamo in modo che produca valori compresi tra 0 e 1, e stabiliamo una soglia per determinare quali valori dovrebbero essere considerati SPAM (ad esempio, se il valore è superiore a 0.5, l'email è spam).
>
>![[Pasted image 20230930130918.png|500]]


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