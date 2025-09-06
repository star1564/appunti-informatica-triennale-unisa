---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Distribuzioni V.A. Discrete
cssclasses:
  - 
---
>La [[024 Funzione di Distribuzione|distribuzione]] **Ipergeometrica** di una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$ rappresenta la *probabilità di $k$ successi* (estrazioni casuali per le quali l'oggetto estratto ha una determinata caratteristica) *in $n$ estrazioni*, *senza reinserimento*, da una *popolazione finita di dimensione $N$* che *contiene esattamente $m$ oggetti con quella caratteristica*, dove ogni estrazione è un successo o un fallimento. 

> [!example]- <font color="orange">Esempio</font>
>Estrarre $n$ biglie senza reinserimento da un'urna che contiene $N$ biglie, di cui $m$ sono bianche (successo) e $N-m$ nere (fallimento). Sia $X$ il numero di biglie bianche presenti tra le $n$ estratte, allora $X$ è detta variabile aleatoria ipergeometrica.
>
>![[Pasted image 20240820165750.png|700]]

In contrasto con la ipergeometrica, la  [[029 Distribuzione Binomiale np|Binomiale np]] descrive la probabilità di $k$ successi in $n$ estrazioni *con reinserimento*. 

#### Densità Discreta
>La sua [[025 Variabili Aleatorie Discrete#Densità Discreta|Densità Discreta]] può essere espressa in questo modo:
>$$\color{#CC241D}p(k)=\frac{{m\choose k} {N-m\choose n-k}}{{N\choose n}},\quad k=0,1,2,\ldots$$



> [!note] Validità
> Attraverso la [[006 Coefficiente Binomiale#Uguaglianza di Vandermonde|formula di Vandermonde]] abbiamo che
> $$\displaystyle\sum_{k=0}^{n}p(k)=\displaystyle\sum_{k=0}^{n}\frac{{m\choose k} {N-m\choose n-k}}{{N\choose n}}=\frac{1}{{N\choose n}}\displaystyle\sum_{k=0}^{n}{m\choose k} {N-m\choose n-k}=\frac{{N\choose n}}{{N\choose n}}=1$$

#### Approssimazione con il Binomiale np
Può essere usata come *approssimazione* di una [[029 Distribuzione Binomiale np|V.A. Binomiale np]] quando $m$ ed $N$ sono grandi rispetto ad $n$ e $k$.


#### Valore Atteso
Il suo [[026 Valore Atteso di una VA Discreta|Valore Atteso]] è il seguente:
$$\color{#CC241D}E[X]=np$$


#### Varianza
La sua [[027 Varianza di una VA Discreta|Varianza]] è la seguente:
$$\color{#CC241D}\text{Var}(X)=np(1-p)\left(1- \frac{n-1}{N-1}\right)$$



___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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
- [Wikipedia](https://en.wikipedia.org/wiki/Hypergeometric_distribution)
- [Video (inglese), Statistics 110 - Ipergeometrica](https://youtu.be/LX2q356N2rU?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=2148)
