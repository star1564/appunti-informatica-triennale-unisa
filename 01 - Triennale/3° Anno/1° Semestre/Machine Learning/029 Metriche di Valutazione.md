---
aliases: 
- Gini Impurity
- Impurità di Gini
- Entropy
- Entropia
- Information Gain
tags:
  - corsi/informatica/machine_learning
paragrafo: Alberi di Decisione
cssclasses:
  - 
---
Quando si seleziona la migliore combinazione di una caratteristica e di un valore come punto di divisione, si possono utilizzare due **Metriche per misurare la qualità di uno split** (separazione).

## Impurità di Gini
>La **Gini Impurity** è una misura del *tasso di impurità nella distribuzione* delle classi di un dataset. 

Supponendo che un insieme di dati abbia $K$ classi, con i dati della classe $k$ che occupano una frazione $f_k$ dell'intero insieme di dati ($0 \leq f_k \leq 1$), la Gini Impurity si calcola come segue:

$$\text{Gini Impurity} = 1 - \sum_{k=1}^{K} f_k^2$$

> [!quote] Criterio
> Una Gini Impurity **più bassa** indica un dataset più *puro*. 

- Quindi, se il set di dati contiene solo una classe, la Gini Impurity sarà zero:  $1 - (1^2 + 0^2 ) = 0$
- Se invece il set di dati è equamente diviso tra due classi, la Gini Impurity sarà un mezzo: $1 -(0,5^2 + 0,5^2) = 1 -(0,25 + 0,25) = 0,5$

Per valutare la *qualità di una suddivisione*, è possibile *sommare la Gini Impurity di tutti i sottogruppi risultanti*, combinando le proporzioni di ciascun sottogruppo come corrispondenti fattori di peso. 

Utilizzando le etichette di un set di dati, è possibile implementare una funzione per calcolare l'impurità di Gini. Anche in questo contesto, una somma ponderata **più bassa** della Gini Impurity indica una *suddivisione migliore*.

> [!example]- <font color="orange">Esempio</font>
>Si veda il seguente esempio di pubblicità di auto a guida autonoma.
>
>Abbiamo scelto di creare un [[028 Alberi di Decisione|Albero di decisione]] a partire da uno dei due seguenti **split iniziali**, ma vogliamo vedere qual è l'approccio migliore.
>
>![[Pasted image 20231216143406.png|800]]
>
>***GINI IMPURITY 1***:
>Ci sono due categorie:
>- <font color="#CC241D">M</font> = 3, quindi sono i 3/5
>	- <font color="#00C575">Click effettuato: 2/3</font>
>	- <font color="#FF6611">Click non effettuato: 1/3</font>
>- <font color="#61AFEF">F</font> = 2, quindi sono i 2/5
>	- <font color="#00C575">Click effettuato: 1/2</font>
>	- <font color="#FF6611">Click non effettuato: 1/2</font>
>
>$$\text{\#1 Gini Impurity} = \textcolor{#CC241D}{\frac{3}{5}}\cdot\left[ 1-\left( \left( \textcolor{#00C575}{\frac{2}{3}} \right)^2 + \left( \textcolor{#FF6611}{\frac{1}{3}} \right)^2\right) \right] + \textcolor{#61AFEF}{\frac{2}{5}}\cdot\left[ 1-\left( \left( \textcolor{#00C575}{\frac{1}{2}} \right)^2 + \left( \textcolor{#FF6611}{\frac{1}{2}} \right) ^2\right) \right] = 0,467$$
>
>***GINI IMPURITY 2***:
>Ci sono due categorie:
>- <font color="#CC241D">True</font> = 2, quindi sono i 2/5
>	- <font color="#00C575">Click effettuato: 2/2 = 1</font>
>	- <font color="#FF6611">Click non effettuato: 0/2 = 0</font>
>- <font color="#61AFEF">False</font> = 3, quindi sono i 3/5
>	- <font color="#00C575">Click effettuato: 1/3</font>
>	- <font color="#FF6611">Click non effettuato: 2/3</font>
>
>
>$$\text{\#2 Gini Impurity} = \textcolor{#CC241D}{\frac{2}{5}}\cdot\left[ 1-\left( \textcolor{#00C575}{1}^2 + \textcolor{#FF6611}{0}^2\right) \right] + \textcolor{#61AFEF}{\frac{3}{5}}\cdot\left[ 1-\left( \left( \textcolor{#00C575}{\frac{1}{3}} \right)^2 + \left( \textcolor{#FF6611}{\frac{2}{3}} \right) ^2\right) \right] = 0,267$$
>
>La suddivisione dei dati in base all'interesse dell'utente per la tecnologia (GINI IMPURITY 2) è una strategia migliore rispetto al genere (GINI IMPURITY 1).

## Information Gain (Entropia)
L'**Information Gain** (guadagno d'informazione) misura il *miglioramento della purezza dopo una suddivisione*, ovvero la riduzione dell'incertezza dovuta alla suddivisione.

> [!quote] Criterio (Information Gain)
> Un Information Gain **più elevato** indica una suddivisione *migliore*. 

Si calcola confrontando l'**Entropia** prima e dopo la suddivisione. Questa, misura l'incertezza di un insieme di dati.
Dato un insieme di dati con $K$ classi e $f_k$ ($0\leq f_{k} \leq 1$) la frazione di dati della classe $k$ ($1\leq k\leq K$), l'entropia dell'insieme dei dati è definita come segue: 
$$\text{Entropy} = -\sum_{k=1}^{K} f_k \cdot \log_{2}f_{k}$$
L'**entropia più bassa** implica un insieme di dati *più puro* e con meno ambiguità.
In un caso perfetto, in cui il set di dati contiene una sola classe, l'entropia è zero. Invece, quando si lancia una moneta, sarà 1.

L'information gain misura la riduzione dell'incertezza dopo lo split, si definisce come la *differenza di entropia prima* dello split (genitore) *e dopo* lo split (figli):
$$\text{Information Gain} = \text{Entropy}(before) - \text{Entropy}(after) = \text{Entropy}(parent)-\text{Entropy}(children)$$

> [!example]- <font color="orange">Esempio</font>
>Durante il processo di costruzione di un nodo di un albero, il nostro obiettivo è cercare il punto di divisione in cui si ottiene il massimo guadagno di informazioni.
>
>Poiché l'entropia del nodo genitore è invariata, è sufficiente misurare l'entropia dei figli risultanti da una divisione.
>$$
>\text{Entropy Before} = -\left(
>        \frac{3}{5} \cdot \log_{2}\frac{3}{5}
>        + \frac{2}{5} \cdot \log_{2}\frac{2}{5}
>    \right)
>= 0,971$$
>
>![[Pasted image 20231216143406.png|800]]
>
>***INFORMATION GAIN 1***:
>Ci sono due categorie:
>- <font color="#CC241D">M</font> = 3, quindi sono i 3/5
>	- <font color="#00C575">Click effettuato: 2/3</font>
>	- <font color="#FF6611">Click non effettuato: 1/3</font>
>- <font color="#61AFEF">F</font> = 2, quindi sono i 2/5
>	- <font color="#00C575">Click effettuato: 1/2</font>
>	- <font color="#FF6611">Click non effettuato: 1/2</font>
>
>$$\text{\#1 Entropy} = 
>    \textcolor{#CC241D}{\frac{3}{5}}\cdot\left[-\left(
>        \textcolor{#00C575}{\frac{2}{3}} \cdot \log_{2}\textcolor{#00C575}{\frac{2}{3}}
>        + \textcolor{#FF6611}{\frac{1}{3}} \cdot \log_{2}\textcolor{#FF6611}{\frac{1}{3}}
>    \right)\right]
>    +
>    \textcolor{#61AFEF}{\frac{2}{5}}\cdot\left[-\left(
>        \textcolor{#00C575}{\frac{1}{2}} \cdot \log_{2}\textcolor{#00C575}{\frac{1}{2}}
>        + \textcolor{#FF6611}{\frac{1}{2}} \cdot \log_{2}\textcolor{#FF6611}{\frac{1}{2}}
>    \right)\right] 
>= 0,951$$
>
>$$\text{\#1 Information Gain} = 0,971 - 0,951 = 0,020$$
>
>
>***INFORMATION GAIN 2***:
>Ci sono due categorie:
>- <font color="#CC241D">True</font> = 2, quindi sono i 2/5
>	- <font color="#00C575">Click effettuato: 2/2 = 1</font>
>	- <font color="#FF6611">Click non effettuato: 0/2 = 0</font>
>- <font color="#61AFEF">False</font> = 3, quindi sono i 3/5
>	- <font color="#00C575">Click effettuato: 1/3</font>
>	- <font color="#FF6611">Click non effettuato: 2/3</font>
>
>$$\text{\#2 Entropy} = 
>    \textcolor{#CC241D}{\frac{2}{5}}\cdot\left[-\left(
>        \textcolor{#00C575}{1} \cdot \log_{2}\textcolor{#00C575}{1}
>        + \textcolor{#FF6611}{0} \cdot \log_{2}\textcolor{#FF6611}{0}
>    \right)\right]
>    +
>    \textcolor{#61AFEF}{\frac{3}{5}}\cdot\left[-\left(
>        \textcolor{#00C575}{\frac{1}{3}} \cdot \log_{2}\textcolor{#00C575}{\frac{1}{3}}
>        + \textcolor{#FF6611}{\frac{2}{3}} \cdot \log_{2}\textcolor{#FF6611}{\frac{2}{3}}
>    \right)\right] 
>= 0,551$$
>
>$$\text{\#2 Information Gain} = 0,971 - 0,551 = 0,420$$
>
>La suddivisione dei dati in base all'interesse dell'utente per la tecnologia (INFORMATION GAIN 2) è una strategia migliore rispetto al genere (INFORMATION GAIN 1).



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