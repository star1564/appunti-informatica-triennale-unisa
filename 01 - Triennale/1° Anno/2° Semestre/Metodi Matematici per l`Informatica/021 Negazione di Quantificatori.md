---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Logica Predicativa
cssclasses:
  - 
---
Si consideri la frase "Tutti gli studenti del corso hanno studiato MMI". Viene [[020 Traduzioni in Forma Simbolica|tradotta in forma simbolica]] con: $\forall x\ P(x)$.

>Per *negare le espressioni* con quantificatori si usano le **[[Leggi De Morgan|Leggi di De Morgan]] per Quantificatori**. Ovvero:
>$$\color{#CC241D}\neg[\forall x\ P(x)]\equiv \exists x\ \neg[P(x)]$$
>$$\color{#CC241D}\neg[\exists x\ P(x)]\equiv \forall x\ \neg[P(x)]$$


Infatti, quando un quantificatore deve essere negato, questo diventa un [[019 Quantificatore Esistenziale|quantificatore esistenziale]] se era un [[018 Quantificatore Universale|quantificatore universale]] e viceversa.

> [!quote] Dimostrazione
>Assumiamo che $\neg[\forall x\ P(x)]$ sia vera.
>
>Quindi $\forall x\ P(x)$ è falsa. Questo sta a significare che esiste un elemento $x$ nel dominio tale che il valore di $P(x)$ sia falsa.
>Quindi, esiste un elemento $x$ nel dominio per il quale $\neg P(x)$ sia vera, cioè $\exists x[\neg P(x)]$ è vera.


> [!example]- <font color="orange">Esempi</font>
>Dimostrare che $\neg[\forall x(P(x)\Rightarrow Q(x))]\equiv \exists x[P(x)\land\neg Q(x)]$.
>
>$\neg[\forall x(P(x)\Rightarrow Q(x))]\equiv$
>$\equiv \exists x\neg[P(x)\Rightarrow Q(x)]\equiv\quad$ (De Morgan per Quantificatori)
>$\equiv\exists x[P(x)\land\neg Q(x)]\quad$ (De Morgan)
>
>---
>Tradurre la frase "Non è vero che tutti gli italiani mangiano la pasta".
>
>Si specifica che: "$x$ è un italiano", $Ita(x)$;
>Si specifica che: "$x$ mangia la pasta", $Pasta(x)$;
>
>Ogni italiano mangia la pasta: $\forall x[Ita(x)\Rightarrow Pasta(x)]$
>
>*Non è vero che* ogni italiano mangia la pasta:
>$\neg[\forall x(Ita(x)\Rightarrow Pasta(x))]\equiv$
>$\equiv\exists x\neg(Ita(x)\Rightarrow Pasta(x))\equiv$
>
>Sappiamo che $\neg(p\Rightarrow q)\equiv p\land\neg q$ perché hanno la stessa tabella di verità, quindi:
>
>$\equiv\exists x[Ita(x)\land\neg(Pasta(x))]$



___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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