---
aliases: 
tags:
  - corsi/informatica/progettazione_algoritmi
paragrafo: Notazioni Asintotiche
cssclasses:
  - 
---
Siano $f$ e $g$ due [[011 Funzioni|funzioni]] dai naturali ai reali positivi, ovvero $f:n\in\mathbb{N}\to f(n)\in\mathbb{R}_+$, $g:n\in\mathbb{N}\to g(n)\in\mathbb{R}_+$.

Diremo che **$\color{#CC241D}f(n)=\Theta(g(n))$**, ovvero che la funzione $f(n)$ è $\Theta(g(n))$, se e solo se esistono tre costanti positive $c_1, c_2,n_0\in \mathbb{N}$ tale che $$\color{#61AFEF}f(n)=O(g(n))\ \land\ f(n)=\Omega(g(n))$$
Ovvero che $\exists c_1, c_2, n_0: c_1\cdot g(n)\leq f(n)\leq c_2\cdot g(n), \forall n\geq n_0$.

> [!tip] Definizione informale
> È vero che $f(n) = \Theta(g(n))$ se la funzione $f(n)$ e la funzione $g(n)$ crescono alla stessa velocità.
>
> La notazione $\Theta$ ne [[006 Insieme Illimitato e Limitato#Insiemi Limitati|limita inferiormente e superiormente]] la crescita e pertanto fornisce una indicazione della *velocità minima e massima* dell'algoritmo.
>
>Graficamente, abbiamo che il grafico della funzione $f(n)$, dal punto $n_0$ in poi, si trova sempre *sopra* il grafico della funzione $c_1\cdot g(n)$ ([[005 Notazione θ|notazione θ]]) e sempre *sotto* il grafico della funzione $c_2\cdot g(n)$ ([[004 Notazione Ω|notazione Ω]]), per ogni $c_1>0, c_2 >0$ .
>
>![[Pasted image 20230311103529.png|700]]

> [!example]- <font color="orange">Esempio</font>
>Proviamo che $4n^n+log\ n=\Theta(n^2)$. Osserviamo che:
>- $\color{#61AFEF}4n^2+log\ n = \Omega(n^2)$, in quanto $4n^2+log\ n\geq n^2$;
>- $\color{#61AFEF}4n^2+log\ n = O(n^2)$, in quanto $4n^2+log\ n\leq 4n^2+n\leq 4n^2+n^2=5n^2$.
>Quindi è vero.




___
[[000 Indice PA|↖ Ritorna all'indice ↖]]

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