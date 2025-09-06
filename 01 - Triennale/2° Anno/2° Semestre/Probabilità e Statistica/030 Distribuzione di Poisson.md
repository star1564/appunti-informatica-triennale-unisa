---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Distribuzioni V.A. Discrete
cssclasses:
  - 
---
>Una [[023 Variabili Aleatorie|Variabile Aleatoria]] $X$ ha la [[024 Funzione di Distribuzione|distribuzione]] di **Poisson** se rappresenta una distribuzione di probabilità che esprime le *probabilità per un numero di eventi* che si verificano *successivamente ed indipendentemente* in un *dato intervallo di tempo*, sapendo che in media se ne verifica un numero $\lambda>0$. 

> [!example]+ <font color="orange">Esempi</font>
>Si utilizza una distribuzione di Poisson per:
>- misurare il numero di chiamate ricevute in un call-center in un determinato arco temporale, come una mattinata lavorativa.
>- misurare il numero di mail ricevute in un'ora

#### Densità Discreta
>La sua [[025 Variabili Aleatorie Discrete#Densità Discreta|Densità Discreta]] può essere espressa in questo modo:
>$$\color{#CC241D}p(k)=e^{-\lambda} \frac{\lambda^k}{k!},\quad k=0,1,2,\ldots$$

Dove:
- $e$ è il [[Numero di Nepero]]
- $p(k)>0, \forall k$

> [!note] Validità
$$\begin{align}\sum_{k=0}^{\infty} p(k)&=\sum_{k=0}^{\infty} e^{-\lambda} \frac{\lambda^k}{k!}\\ &=e^{-\lambda} \sum_{k=0}^{\infty} \frac{\lambda^k}{k!}\\ &=e^{-\lambda}e^\lambda\\ &=e^0 \\ &=1\end{align}$$
>
>Dove $\displaystyle\sum_{k=0}^{\infty} \frac{\lambda^k}{k!}=e^\lambda$.

#### Approssimazione con il Binomiale np
Può essere usata come *approssimazione* di una [[029 Distribuzione Binomiale np|V.A. Binomiale np]] quando:
- $n$ è grande
- $p$ è piccolo
- $np = \lambda > 0$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240820145806.png]]

#### Valore Atteso
Il suo [[026 Valore Atteso di una VA Discreta|Valore Atteso]] è il seguente:
$$\color{#CC241D}E[X]=\lambda$$

> [!quote] Dimostrazione
>$$\begin{align}E[X]&=e^{-\lambda}\cdot\sum_{k=0}^{\infty}\left( k\cdot \frac{\lambda^k}{k!} \right)\\ &=e^{-\lambda}\cdot\sum_{k=1}^{\infty}\left( \frac{\lambda^k}{(k-1)!} \right)\\ &= e^{-\lambda}\cdot\lambda\cdot\sum_{k=1}^{\infty}\left( \frac{\lambda^{k-1}}{(k-1)!} \right)\\ &=e^{-\lambda}\cdot e^\lambda \cdot \lambda \\ &=\lambda\end{align}$$

#### Varianza
La sua [[027 Varianza di una VA Discreta|Varianza]] è la seguente:
$$\color{#CC241D}\text{Var}(X)=\lambda$$


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
- [Wikipedia](https://it.wikipedia.org/wiki/Distribuzione_di_Poisson)
- [Video (inglese), Statistics 110 - Poisson](https://youtu.be/TD1N4hxqMzY?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=370)