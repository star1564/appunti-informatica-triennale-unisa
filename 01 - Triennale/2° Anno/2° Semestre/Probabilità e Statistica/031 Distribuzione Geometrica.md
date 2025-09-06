---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Distribuzioni V.A. Discrete
cssclasses:
  - 
---
>La [[024 Funzione di Distribuzione|distribuzione]] **Geometrica** di una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$ rappresenta il *numero di fallimenti ottenuti prima del primo successo* durante delle prove indipendenti sulla [[028 Distribuzione di Bernoulli|Bernoulli]].


#### Densità Discreta
>La sua [[025 Variabili Aleatorie Discrete#Densità Discreta|Densità Discreta]] può essere espressa in questo modo:
>$$\color{#CC241D}p(n)=(1-p)^{n-1}\cdot p$$
>Per $n=1,2,\dots$

> [!quote] Dimostrazione
>Si ha $X=n$ se le prime $n-1$ prove siano state un fallimento e l'$n$-esima prova un successo, quindi per l'[[021 Eventi Indipendenti|indipendenza tra gli eventi]] $A_k=\{\text{successo alla prova }k\text{-esima}\}$, si ha che 
>
>$$\begin{align}p(n)&=\mathbb{P}(\overline{A_1} \cap \overline{A_2}\cap \ldots\cap\overline{A_{n-1}}\cap A_n)\\ &=\mathbb{P}(\overline{A_1}) \cdot \mathbb{P}(\overline{A_2})\cdot \ldots\cdot\mathbb{P}(\overline{A_{n-1}}) \cdot \mathbb{P}(A_n)\\ &=(1-p)^{n-1}\cdot p\end{align}$$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240820161404.png|700]]

> [!note] Validità
> $$\displaystyle\sum_{n=1}^{\infty} p(n)= p\displaystyle\sum_{n=1}^{\infty} (1-p)^{n-1}=\frac{p}{1-(1-p)}=1$$

#### Valore Atteso
Il suo [[026 Valore Atteso di una VA Discreta|Valore Atteso]] è il seguente:
$$\color{#CC241D}E[X]=\frac{1}{p}$$

> [!quote] Dimostrazione
> Posto $q=1-p$ si ha che:
>$$\begin{align}E[X]&=\sum_{n=0}^{\infty} \Big(n\cdot [q^{n-1}\cdot p]\Big)\\ &= p\cdot\sum_{n=0}^{\infty} \Big(n\cdot [q^{n-1}]\Big) \\ &= p\cdot\sum_{\color{#61AFEF}n=1}^{\infty} \left(\frac{d}{dq} (q^{n})\right)\\ &= p\cdot \frac{d}{dq}\cdot\left(\sum_{n=0}^{\infty} q^n\right)\end{align}$$
>
> Utilizzando l'identità $\displaystyle\sum_{n=0}^{\infty} q^n= \frac{1}{1-q}$ ricaviamo il valore atteso
> $$p\cdot\frac{d}{dq}\cdot\left( \frac{1}{1-q} \right) = \left( \frac{p}{(1-q)} \right)^2 = \frac{1}{p}$$

#### Varianza
La sua [[027 Varianza di una VA Discreta|Varianza]] è la seguente:
$$\color{#CC241D}\text{Var}(X)=\frac{1-p}{p^2}$$

> [!quote] Dimostrazione
>Posto $q=1-p$, calcoliamo il momento del secondo ordine di $X$:
>
>$$\begin{align}E[X^2]&=\sum_{n=0}^{\infty} (n^2\cdot [q^{n-1}\cdot p])\\ &=p\cdot\sum_{n=1}^{\infty} \left(\frac{d}{dq}[n\cdot q^n]\right)\\ &= p\cdot\frac{d}{dq}\cdot\left( \sum_{n=1}^{\infty}(n\cdot q^n)\right)\\ &= p\cdot\frac{d}{dq}\cdot\left(\frac{q}{p}\sum_{n=1}^{\infty} n q^{n-1}p\right)\end{align}$$
>
>Dato che $\sum_{n=1}^{\infty} n\cdot q^{n-1} p=\frac{1}{p}$ si ha che
>$$p \frac{d}{dq}\left( \frac{q}{(1-q)^2} \right)=p\left( \frac{(1-q)^2 + 2q(1-q)}{(1-q)^4} \right)=p\left( \frac{1}{p^2} + \frac{2(1-p)}{p^3} \right) = \frac{2}{p^2}-\frac{1}{p}$$
>
>Quindi abbiamo che $$\text{Var}(X)=E(X^2)- (E(X))^2 = \frac{2}{p^2}-\frac{1}{p} - \frac{1}{p^2}= \frac{1-p}{p^2}$$

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
- [Video (inglese), Statistics 110 - Geometrica](https://youtu.be/LX2q356N2rU?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=2469)