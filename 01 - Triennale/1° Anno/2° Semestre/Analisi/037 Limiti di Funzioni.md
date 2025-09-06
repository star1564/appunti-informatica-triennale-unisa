---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Funzioni
cssclasses:
  - 
---
## Limite Finito
Sia $f:A\subseteq\mathbb{R}\to\mathbb{R}$ una [[011 Funzioni|funzione]] e sia $x_0\in\mathbb{R}$ un [[009 Intorno di un Punto#Punto di Accumulazione|punto di accumulazione]] di $A$.

>Diciamo che il **[[017 Limiti|limite]] della funzione $f(x)$** tende al valore $c$ al tendere di $x$ ad $x_0$, e si scrive $$\color{#61AFEF}\lim\limits_{x \to x_0}f(x)=L\in\mathbb{R}$$
>se, **per ogni** [[009 Intorno di un Punto|intorno]] $V(L, \epsilon)$ esiste un introno $U(x_0, \delta)$ tale che $$x\in \textcolor{green}{U(x_0, \delta)}\ \cap\ A-\{x_0\}\Rightarrow f(x)\in \color{#CC241D}V(L, \epsilon)$$

Ovvero, $\lim f(x_0)=L$ se, per ogni intorno di $L$, si riesce a trovare un intorno di $x_0$ tale che, se si calcolano i valori che la funzione assume nei punti di questo intervallo diversi dal punto $x_0$, questi valori appartengono all'introno di $L$ che stavamo considerando.

Questo deve essere vero per ogni intervallo.

![[Pasted image 20220405113421.png|600]]

> [!note] Nota
> Non è importante capire cosa fa la funzione esattamente sul punto $x_0$, ma cosa fa quando ci si avvicina al punto $x_0$.
> Man mano che ci si avvicina ad $x_0$, i valori assunti dalla funzione si avvicinano ad $L$.



## Limiti Infiniti
A seconda di $x_0$ e $L$, che possono essere numeri reali o $\pm\infty$, esplicitando gli intorni si ottengono le definizioni in forma $\epsilon-\delta$.

>Sia $f:A\subseteq\mathbb{R}\to\mathbb{R}$. Diciamo che la funzione $f(x)$ tende a $+\infty$, ovvero $$\color{#61AFEF}\lim\limits_{x \to +\infty}f(x)=+\infty$$
>se, per ogni $M>0$ esiste $K>0$ tale che $$x\in\textcolor{green}{(K,+\infty)}\Rightarrow \color{#CC241D}f(x)>M$$

![[Pasted image 20220405115708.png|680]]

Ovvero, se esiste un $x$ che si trova nell'[[009 Intorno di un Punto#INTORNO DI infty|intorno infinito]] di $+\infty$, la sua immagine $f(x)=y$ deve essere maggiore di $M$.

La situazione è analoga per $-\infty$.

___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
- [Elia Bombardelli](https://youtu.be/nnpUv-WEQpE?si=qPpWoI_jYu60P8Fo)