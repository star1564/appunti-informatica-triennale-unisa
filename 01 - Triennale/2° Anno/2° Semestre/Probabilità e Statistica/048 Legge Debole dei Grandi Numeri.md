---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Teoremi Limite
cssclasses:
  - 
---
Sia $X_1,X_2,\dots$ una successione di variabili identicamente distribuite, ognuna con [[026 Valore Atteso di una VA Discreta|valore atteso]] finito $E[X_i]=\mu$. 
>La **Legge Debole dei Grandi Numeri** dice che, posto 
>$$\overline{X}_{n}=\frac{1}{n}\sum_{i=1}^{n}X_{i}$$
>per ogni $\epsilon > 0$, si ha che 
>$$\lim\limits_{n \to \infty}\mathbb{P}(|\overline{X}-\mu|\geq \epsilon)=0$$

La legge debole dei grandi numeri afferma che, aumentando il numero $n$ di prove indipendenti di una variabile aleatoria con la stessa distribuzione, la [[045 Valore Atteso di Somme di Variabili Aleatorie#Media Campionaria|Media Campionaria]] si avvicina sempre di più alla media teorica della distribuzione.

Ovvero:
- La media delle osservazioni *diventa più precisa con l'aumentare di $n$*.
- Più grande è $n$, più spesso la media campionaria sarà vicina alla media vera.

![[Pasted image 20250323120516.png|700]]

Equivalentemente, per ogni $\epsilon>0$, risulta che 
$$\operatorname*{lim}_{n\to\infty}\mathbb{P}(\mu-\epsilon\lt \overline{X}_{n}\lt \mu+\epsilon)=\operatorname*{lim}_{n\to\infty}\mathbb{P}(|\overline{X}_{n}-\mu|\lt \epsilon)=1$$

> [!quote] Dimostrazione
>Assumiamo che $\text{Var}(X_i)=\sigma^2$ sia finita. Poiché $E[\overline{X_n}]=\mu$ e $\text{Var}(\overline{X}_n)=\frac{\sigma^2}{n}$, dalla [[047 Disuguaglianza di Chebyshev|Disuguaglianza di Chebyshev]] si ha, per ogni $\epsilon> 0$
>$$\mathbb{P}(|\overline{X}_{n}-\mu|\geq\epsilon)\leq{\frac{\sigma^{2}}{n}}{\frac{1}{\epsilon^{2}}}$$
>
>Procedendo al [[037 Limiti di Funzioni|limite]] si ha 
>$$\lim\limits_{n \to \infty}\mathbb{P}(|\overline{{{X}}}_{n}-\mu|\geq\epsilon)\leq\lim\limits_{n \to \infty}{\frac{\sigma^{2}}{n}}{\frac{1}{\epsilon^{2}}}=0$$

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
- https://it.wikipedia.org/wiki/Legge_dei_grandi_numeri