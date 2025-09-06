---
aliases:
  - ε-chiusura
  - epsilon-chiusura
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Non Deterministici
cssclasses:
  - 
---
>La **$\epsilon$-chiusura di uno stato** di un [[009 Automi Finiti Non Deterministici|NFA]] trova *tutti gli stati raggiungibili* dallo stato $q$ attraverso cammini *etichettati solamente da $\epsilon$-transazioni*. In questo insieme viene inserito anche lo stato $q$ stesso.

Viene indicata con $E(q)$ oppure $ECLOSE(q)$.

> [!quote] Definizione Formale [HUM, 2.5.3]
>Sia $A=(Q,\Sigma,\delta,q_0,F)$ un automa finito non deterministico e sia $q\in Q$ uno stato. La $\epsilon$-chiusura $\color{#CC241D}E(q)$ di $q$ è un sottoinsieme di $Q$ definito ricorsivamente come segue
>- **Passo Base**: 
>$$\color{#61AFEF}q\in E(q)$$
>- **Passo Ricorsivo**: $\forall p \in E(q)$,
>$$\color{#61AFEF}\delta(p,\epsilon)\subseteq E(q)$$
>
>Inoltre, sia $R\subseteq Q$. La $\epsilon$-chiusura $E(R)$ di $R$ è
>$$E(R)=\bigcup_{q\in R} E(q)$$
>Quindi, per ogni stato in $R$, considero le $\epsilon$-chiusure di ogni stato.

Inoltre $E(\emptyset)=\emptyset$.


> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240320152718.png]]
>
>- $E(q_2)=\{q_2\}$
>
>‎ %%← Empty Char%%
>
>- $E(q_1)=\{q_1\}\cup E(q_2)$
>$= \{q_1\}\cup \{q_2\}$
>$=\{q_1,q_2\}$
>
>‎ %%← Empty Char%%
>
>- $E(q_0)=\{q_0\} \cup E(q_1)=$
>$=\{q_0\} \cup \{q_1\} \cup E(q_2)=$ [passo base]
>$=\{q_0\} \cup \{q_1\} \cup \{q_2\}$
>$=\{q_0,q_1,q_2\}$
>
>---
>![[Pasted image 20240320153229.png]]


___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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