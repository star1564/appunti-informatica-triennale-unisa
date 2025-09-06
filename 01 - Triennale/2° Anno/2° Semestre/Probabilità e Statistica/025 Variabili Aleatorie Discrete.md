---
aliases:
  - Funzione di Probabilità
  - Densità Discreta
  - Probability Mass Function
  - PMF
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: V.A. Discrete
cssclasses:
  - 
---
>Una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$ viene detta **Discreta** se essa può assumere un valore appartenente allo [[010 Spazio Campionario|Spazio Campionario]] composto da un numero *finito* di valori possibili.

Quindi è una variabile aleatoria in cui si possono listare i valori possibili.

## Densità Discreta
>Definiamo inoltre come **densità discreta** (o *funzione di probabilità*, Probability Mass Function, PMF) la funzione che associa una probabilità ad un qualsiasi valore possibile di $X$. $$\color{#CC241D}p(k)=\mathbb{P}(X=k)$$

>[!warning] È diversa dalla funzione di distribuzione
> La [[024 Funzione di Distribuzione|funzione di distribuzione]] utilizza $X\leq k$, invece la densità discreta utilizza $X = k$.
> Inoltre, la funzione di distribuzione è utile per qualsiasi tipo di variabile aleatoria, mentre la funzione di probabilità è utile solo per le variabili aleatorie discrete.

>Una densità discreta soddisfa le seguenti condizioni:
>1. La probabilità, se $X$ assume i valori $x_1, x_2, \dots, x_i$, sarà la seguente $$\color{#61AFEF}p(x_i)=\begin{cases} \geq0,\ \text{se}\ i=1,2,\dots \\ =0,\ \text{altrimenti} \end{cases}$$
>2. Poiché $X$ deve assumere almeno uno dei valori $x_i$, si ha che somma dei valori della funzione di probabilità $p(x)$ è $\color{#61AFEF}1$. $$\color{#e86162}\displaystyle\sum_{i} p(x_i) = 1$$ ^308110

Può essere utile rappresentare la densità discreta in forma grafica ponendo i valori $x_i$ in ascissa e $p(x_i)$ in ordinata.

> [!example]+ <font color="orange">Esempio</font>
>La variabile aleatoria $X$ mostra il punteggio ottenuto lanciando due dadi.
>Lo spazio degli esiti $S$ è composto da 36 esiti: $S=\{(1,1),(1,2),\dots,(6,6)\}$.
>
>Gli eventi possibili dell'esperimento sono 11. Ogni evento è il punteggio ottenuto sommando i due dadi:
>$X=\{2,3,4,5,6,7,8,9,10,11,12\}$
>
>La densità discreta di ogni evento è la seguente:
>
>![[Pasted image 20230523114338.png|800]]
>
>La somma di tutte le probabilità nella densità discreta è pari a 1.

La densità discreta consente di calcolare la probabilità che la variabile aleatoria discreta assuma valori *in un sottoinsieme qualsiasi* $B$ di $\mathbb{R}$ attraverso la sommatoria. 
$$\mathbb{P}(X\in B)=\displaystyle\sum_{x_k\in B} p(x_k),\quad \mathbb{P}(a\leq X\leq b) = \displaystyle\sum_{a\leq x_k \leq b} p(x_k)$$

In particolare, se $B=(-\infty, x]$ si ha che la [[024 Funzione di Distribuzione|funzione di distribuzione]] di una variabile aleatoria discreta $X$ può essere espressa come 
$$F(x)=\mathbb{P}(X\leq x)=\mathbb{P}(X\in (-\infty, x]) = \displaystyle\sum_{x_k \leq x} p(x_k),\quad x\in \mathbb{R}$$



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
- [Andrea Minini](https://www.andreaminini.org/statistica/probabilita/variabili-aleatorie)
- [Video (inglese), Statistics 110 - Variabili Aleatorie Discrete](https://youtu.be/k2BB0p8byGA?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=814)