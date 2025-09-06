---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: V.A. Continue
cssclasses:
  - 
---
>Se $X$ è una [[034 Variabili Aleatorie Continue|variabile aleatoria continua]] con [[034 Variabili Aleatorie Continue#Funzione di Densità|funzione di densità]] $f(x)$, il **Valore Atteso** (o valore medio) di $X$ è definito da 
>$$\color{#CC241D}E[X]=\int_{-\infty}^{\infty} x\cdot f(x)dx$$con $f(x)\geq0$ per ogni $x$.

^49886a

> [!example]- <font color="orange">Esempio</font>
>>Determinare $E[X]$ sapendo che la densità di $X$ è $$f(x)=\begin{cases} 2x & 0\leq x\leq 1 \\ 0& \text{altrimenti} \end{cases}$$
>
>$$E[X]=\int_0^1 x\cdot f(2x)dx=2\int_0^1 x^2 dx=2\left[ \frac{x^3}{3} \right]_0^1=\frac{2}{3}$$

### Valore Atteso per una funzione di una Variabile Aleatoria
>Se $X$ è una variabile aleatoria continua con densità $f(x)$, allora per ogni *funzione a valori reali $g(x)$* risulta che 
>$$\color{#61AFEF}E[g(X)]=\int_{-\infty}^{\infty} g(x)f(x)dx$$


> [!example]- <font color="orange">Esempio</font>
>>Determinare $E[e^X]$ sapendo che la densità di $X$ è $$f_X(x)=\begin{cases} 1 & 0\leq x\leq 1 \\ 0& \text{altrimenti} \end{cases}$$
>
>$$E[Y]=E[e^X]=\int_{-\infty}^{\infty} e^x f_X(x)dx=\int_0^1 e^x dx=[e^x]_0^1=e-1= 1,71828$$

### Proprietà di Linearità
>Sia $X$ una variabile aleatoria continua. Se $a$ e $b$ sono costanti reali, allora
>$$\color{#CC241D}E[a\cdot X + b]=a\cdot E[X] + b$$

### Valori compresi tra due numeri
![[026 Valore Atteso di una VA Discreta#^1ddd5c]]

> [!quote] Dimostrazione
> La dimostrazione è analoga a quella del [[026 Valore Atteso di una VA Discreta#^0f1f1c|caso discreto]].

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
- [[026 Valore Atteso di una VA Discreta]]