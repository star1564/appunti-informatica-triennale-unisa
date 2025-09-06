---
aliases: [Insieme Ordinato, Elementi Confrontabili, Insieme Totalmente Ordinato, Catena, Insieme Ben Ordinato]
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Ordine
cssclasses:
  - 
---
Sia $S$ un insieme non vuoto ed $R$ una [[025 Relazioni Binarie|relazione binaria]].
Una [[029 Relazioni d_Ordine|Relazione d'Ordine]] può essere classificata in diverse categorie in base alle sue proprietà specifiche.

### Insieme Ordinato
Un insieme si dice **ordinato** se nella coppia ($S$, $R$) *si ha una relazione d'ordine* in $S$.

### Elementi Confrontabili
Sia ($S$, $R$) un *insieme ordinato*.
Due elementi $x,y\in S$ sono detti **confrontabili** se e solo se:
$$xRy \lor yRx$$

### Insieme Totalmente Ordinato (Catena)
Un insieme ($S$, $R$) si dice **totalmente ordinato (catena)** se e solo se:
1. ($S$, $R$) è un *insieme ordinato*;
2. (Ogni coppia di $S$) $\forall x,y \in S: x,y$ sono *confrontabili*.

### Insieme Ben Ordinato
Un insieme ($S$, $R$) si dice **ben ordinato** se e solo se *ogni sottoinsieme non vuoto di $S$ possiede un [[034 Minimo (MD)|minimo]]*, ovvero 
$$\forall T\subseteq S, T\neq \emptyset, \exists min\ T$$
Quindi basta almeno un sottoinsieme di $S$ in cui non esiste minimo per vedere che l'insieme non è ben ordinato.

Inoltre **ogni insieme ben ordinato è anche totalmente ordinato**.

<font color="green">Dimostrazione (pL 95-96, 2.4.12) CHIEDE SPESSO ALL'ORALE</font>.

($S$, $\leq$) ben ordinato $\Longrightarrow$ ($S$, $\leq$) è totalmente ordinato.

> [!quote] Dimostrazione
>$\forall x, y \in S: \{x, y\}\neq \emptyset$ poiché ($S$, $R$) è ben ordinato $\exists min\ \{x, y\}$.
>- Se $min\ \{x, y\} = x$ allora $x\leq y$;
>- Se $min\ \{x, y\} = y$ allora $y\leq x$.





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
- Pagina Libro: 