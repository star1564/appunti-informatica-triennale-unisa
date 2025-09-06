---
aliases:
  - Gaussiana
  - Variabile Aleatoria Standard
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Distribuzioni V.A. Continue
cssclasses:
  - 
---
>La [[024 Funzione di Distribuzione|distribuzione]] **Normale** (o *Gaussiana*) è spesso usata come prima approssimazione per descrivere una [[034 Variabili Aleatorie Continue|variabile aleatoria continua]] con valori che *tendono a concentrarsi attorno a un singolo [[035 Valore Atteso di una VA Continua|valore atteso]]*.

#### Funzione di Densità
>La sua [[034 Variabili Aleatorie Continue#Funzione di Densità|Funzione di Densità]] può essere espressa in questo modo:
>$$\color{#CC241D}f(x)={\frac{1}{\sqrt{2\pi\sigma^{2}}}}\,e^{-(x-\mu)^{2}/(2\sigma^{2})}={\frac{1}{\sqrt{2\pi\sigma^{2}}}}\exp\left\{-{\frac{(x-\mu)^{2}}{2\sigma^{2}}}\right\}$$
>Con: 
>- $\mu\in\mathbb{R}$ 
>- $\sigma>0$
>- $-\infty<x<\infty$ 

![[Pasted image 20240821121248.png|550]]

La densità è una funzione con *grafico a campana*. È simmetrica rispetto a $x = \mu$, ossia risulta $f(\mu−t) = f(\mu+t)$ per ogni $t \geq 0$. 
Possiede massimo in $\mu$ e due [[057 Derivata Seconda#PUNTI DI FLESSO|punti di flesso]]: $\mu - \sigma$ e $\mu + \sigma$.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240821123943.png]]

#### Valore Atteso
Il suo [[035 Valore Atteso di una VA Continua|Valore Atteso]] è il seguente:
$$\textcolor{#CC241D}{E[X]=\mu}$$


#### Varianza
La sua [[036 Varianza di una VA Continua|Varianza]] è la seguente:
$$\textcolor{#CC241D}{\text{Var}(X)=\sigma^2}$$


#### Variabile Aleatoria Standard
>Una variabile aleatoria continua $Z$ è detta **standard** se $$\color{#61AFEF}E[Z]=0,\quad \text{Var}(Z)=1$$

Se $X$ è una variabile normale di parametri $\mu$ e $\sigma^2$, allora le seguenti sono variabili normali di parametri 0 e 1, ovvero *variabili normali standard*: $$\text{(0) } Z=\frac{X-\mu}{\sigma},\quad \text{(1) } Z'=\frac{\mu-X}{\sigma}$$

La funzione di distribuzione di una variabile normale standard $Z$ si indica con:
$$\Phi(x)=P(Z\leq x)={\frac{1}{\sqrt{2\pi}}}\int_{-\infty}^{x}e^{-z^{2}/2}\,d z,\quad -\infty<x<\infty$$
![[Pasted image 20240821123525.png]]



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
- [Wikipedia](https://it.wikipedia.org/wiki/Distribuzione_normale)
- [Video (inglese), Statistics 110 - Normale](https://youtu.be/72QjzHnYvL0?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=1377)