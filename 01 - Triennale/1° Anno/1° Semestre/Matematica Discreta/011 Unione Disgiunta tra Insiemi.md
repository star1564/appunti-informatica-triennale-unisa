---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Teoria degli Insiemi - Operazioni
cssclasses:
  - 
---
>L'**Unione Disgiunta** è un'operazione che consente di *combinare due insiemi in modo tale che siano completamente separati* in un nuovo insieme. Quindi, dati due insiemi qualsiasi $S$ e $T$, l'intersezione di questi è l'insieme degli elementi che *appartengono all'[[008 Unione tra Insiemi|unione]] di S e T ma non alla loro [[009 Intersezione tra Insiemi|intersezione]]*.
>$$S \textcolor{red}\uplus T := \{x\space|\space x\in (S\cup T)-(S\cap T)\}$$

![[d5598cfe-060d-4e35-a72e-6484be281b02.png|400]]

Dove:
$x \in S\uplus T = (x\in S \land x\notin T)\lor (x\notin S\land x\in T)$ 

$x \notin S\uplus T = \neg((x\in S \land x\notin T)\lor (x\notin S\land x\in T)) =$ $=\neg(x\in S \land x\notin T)\land \neg(x\notin S\land x\in T) =$ $=(x\notin S \lor x\in T)\land (x\in S\lor x\notin T)$.

Si può notare che:
$S \uplus T =(S-T)\cup (T-S)$ <font color="green">Dimostrazione (pL 35, 1.4.19)</font>

> [!example]+ <font color="orange">Esempio</font>
>A = {1, 2, -3, a, b}, B = {b, c, -3}: $\quad$A $\uplus$ B = \{1, 2, a, c\}

#### PROPRIETÀ
- **Commutativa**: $S\uplus T = T\uplus S$;

- **Associativa**: $(S\uplus T)\uplus V = S\uplus(T\uplus V)$;
- L'**Elemento Neutro** rispetto all'unione disgiunta è $\emptyset$;
- $S\uplus S = \emptyset$;
- $\cap$ è **distributiva** rispetto a $\uplus$ ma NON viceversa: $\quad$$S\cap (T\uplus V) = (S\cap T)\uplus (S\cap V)$;

___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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