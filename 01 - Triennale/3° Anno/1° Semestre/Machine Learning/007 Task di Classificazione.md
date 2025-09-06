---
aliases:
  - Classificazione ML
  - Classificazione Binaria
  - Classificazione Multi-classe
  - Classificazione Multi-label
  - Classificazione Multi-classe a Bassa cardinalità
  - Classificazione Multi-classe ad Alta cardinalità
tags:
  - corsi/informatica/machine_learning
paragrafo: Design di Sistemi ML
cssclasses:
  - 
---
>La **classificazione** è una tecnica di machine learning in cui l'obiettivo è assegnare una *categoria* a un insieme di dati in base alle loro caratteristiche.


![[Pasted image 20230930123614.png|400]]

La classificazione si dirama in altri tipi di task, in base al numero di classi da classificare.

> [!quote] Definizione
> Dato un insieme di dati di addestramento contenente osservazioni e i relativi output, l'obiettivo della classificazione è imparare una regola generale che mappi correttamente le osservazioni (chiamate anche [[021 Features Engineering#Feature|Feature]] o variabili predittive) alle categorie di destinazione (chiamate anche [[016 Etichettatutra#Etichetta|Etichette]] o classi). 

## Classificazione Binaria

Nella **Classificazione Binaria**, ci sono *solo due classi* possibili. Questo è il tipo più semplice. 
Ogni oggetto *appartiene esattamente ad una sola classe*.

> [!example]- <font color="orange">Esempio</font>
> Ad esempio, classificare una mail come "spam" o "non spam" è un problema di classificazione binaria.
> 
> ![[Pasted image 20231121113049.png|600]]


## Classificazione Multi-classe

Nella **Classificazione Multi-classe**, ci sono *più di due classi* possibili.
Ogni oggetto *appartiene esattamente ad una sola classe*.

> [!example]- <font color="orange">Esempio</font>
>Ad esempio, classificare dei disegni in "croci", "cerchi", "triangoli", ecc., è un problema di classificazione multi-classe.
>
>![[Pasted image 20231121113158.png|400]]

### Alta e Bassa Cardinalità
La classificazione Multi-classe si divide in: 
- **Bassa Cardinalità**, numero di classi *basso*;
- **Alta Cardinalità**, numero di classi *alto*.

>Più classi ci sono, più complesso diventa il problema.

In generale, i modelli di ML hanno bisogno di *almeno 100 esempi per ogni classe* per imparare a classificare quella classe. Quindi per le classi rare, la raccolta dei dati può essere particolarmente difficile.

Con i task ad alta cardinalità si potrebbe impiegare la **classificazione gerarchica**, dove si impiega inizialmente un classificatore che distingue le istanze in diversi *macrogruppi*. Successivamente, ulteriori classificatori vengono utilizzati per *classificare i sottogruppi*.

> [!example]- <font color="orange">Esempio</font>
>Un primo classificatore distingue i prodotti in macrogruppi generali (elettronica, abbigliamento, alimentari). Per ogni macrogruppo, vengono usati modelli per sottocategorie specifiche (es. smartphone, laptop, televisori per l'elettronica).


## Multi-etichetta

Nella **Classificazione Multi-etichetta**, *un oggetto* può essere associato a *più di una classe*. Questo viene rappresentato di solito attraverso un [[055 Spazi Vettoriali#^da02d3|vettore]], dove ogni numero rappresenta l'appartenenza ad una classe specifica. Il valore numerico si trova nell'[[004 Intervalli#Intervallo Chiuso|intervallo]] $[0,1]$.

> [!example]- <font color="orange">Esempio</font>
>Ad esempio, quando si costruisce un modello per classificare immagini in cinque categorie: [cani, gatti, cavalli, pesci, uccelli], un'immagine può essere categorizzata sia come gatti che come uccelli contemporaneamente, ottenendo un vettore $[0,1,0,0,1]$, dove $1$ rappresenta il fatto che è di quella etichetta specifica, $0$ altrimenti.
>
>![[Pasted image 20231121113335.png|300]]


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