---
aliases:
tags:
  - corsi/informatica/machine_learning
paragrafo: Introduzione
cssclasses:
  - 
---
>Il **Machine Learning** è un approccio che consente ai computer di *apprendere pattern complessi da dati esistenti* e utilizzare questi modelli per fare *predizioni su dati futuri*. 

Questa tecnologia è ampiamente utilizzata sia nell'ambito aziendale, come nella produzione, che nella ricerca, con diverse applicazioni in campi diversi, come la performance del [[Algoritmo e Modello ML|modello]].

![[Pasted image 20231121104250.png|550]]


> [!example]- <font color="orange">Esempio</font>
>Alcune delle applicazioni comuni del machine learning includono:
>- Profilazione
>- Motori di Ricerca con Risultati Pertinenti (sistemi di raccomandazione basati sulle preferenze degli utenti)
>- Predictive Typing (suggerimento di parole basate sul contesto e sulla probabilità)
>- Traduzione Automatica (analisi di pattern e strutture linguistiche)
>- Identificazione e Verifica di Persone (riconoscimento di volti e biometrica)
>- Chatbot (comprensione del linguaggio naturale)
>- Rilevamento di Spam o Frodi


### Quando Utilizzare il Machine Learning
Il machine learning ha senso di essere utilizzato in determinate circostanze:
- **Presenza di Pattern Utili**: Il machine learning è appropriato solo quando ci sono pattern utili nei dati, e il problema da risolvere è *complesso e richiede predizioni*. È particolarmente efficace per *problemi ripetitivi* o quando una predizione errata ha un impatto limitato ed è necessario operare su larga scala.
- **Disponibilità di un [[Dataset]] di Training**: È fondamentale disporre di un dataset di training significativo, sia in termini di *quantità* che di *qualità* dei dati. Inoltre, questi dati devono essere coerenti con quelli che si desidera predire. È importante evitare il [[Bias]] nella raccolta dei dati.
- **Alta Quantità e Qualità di Dati**: In generale, un *maggiore volume di dati* di training porta a una maggiore precisione nelle predizioni e a una migliore generalizzazione.
- **Assunzioni sui Pattern dei Dati**: Per verificare se i dati da predire seguono lo stesso pattern dei dati di training, spesso si fanno delle assunzioni. Se queste assunzioni si rivelano corrette, il modello avrà una *base solida su cui fare predizioni*; in caso contrario, sarà necessario *acquisire nuovi dati di training*.

Quando si progetta un modello *evitare soluzioni hardcoded*, poiché i pattern nei dati possono cambiare nel tempo e diventare obsolete.

### Quando Non Utilizzare il Machine Learning
Il machine learning NON è appropriato in alcune situazioni:
- **Etica**: Non dovrebbe essere utilizzato quando comporta decisioni o azioni *eticamente discutibili*.
- **Esistono soluzioni più semplici**: Se esistono soluzioni più semplici ed *efficaci* per risolvere un problema, il machine learning potrebbe non essere necessario.
- **Economicamente non Conveniente**: L'uso del machine learning può essere *costoso*, quindi non è economicamente conveniente se i benefici non superano i costi.

## Differenze tra ML Aziendale e di Ricerca
La maggior parte dei casi d'uso del ML riguarda *l'ambito aziendale* e quello *di ricerca*. Le differenze sono descritte in questa tabella.

|                             | *Ricerca*                                             | *Azienda*                                      |
| --------------------------- | --------------------------------------------------- | -------------------------------------------- |
| Requisiti                   | Alta performance su datasets                        | Diversi investitori hanno diversi requisiti  |
| Priorità della Computazione | Apprendimento veloce, alta velocità di elaborazione | Predizioni veloci con tempi di latenza bassi |
| Dati                        | Statici                                             | Dinamici                                     |
| Etica                       | Non considerata                                      | Considerata                                  |
| Interpretabilità            | Non considerata                                     | Considerata                                  |

## Tipi di apprendimento
Le tecniche di machine learning possono essere categorizzate in base a diversi criteri:
- [[002 Apprendimento con Supervisione Umana|Apprendimento con Supervisione Umana]];
- [[003 Apprendimento in modo Incrementale|Apprendimento in modo Incrementale]];
- [[004 Apprendimento con Predizioni|Apprendimento con Predizioni]].

> [!note] Nota
>Queste categorie non sono mutualmente esclusive, e spesso i modelli di machine learning possono combinare approcci diversi per affrontare compiti specifici.


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