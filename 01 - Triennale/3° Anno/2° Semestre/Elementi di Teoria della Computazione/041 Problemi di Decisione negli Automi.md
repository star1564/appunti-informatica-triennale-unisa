---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Indecidibilità
cssclasses:
    - 
---

Ci sono tre tipi principali di [[001 Logica Proposizionale#^proposizione|Problema di Decisione]] che si applicano sugli [[004 Automi Finiti Deterministici|automi finiti deterministici]].

Visto che sono deterministici, questi raggiungono sempre uno stato finale, quindi sono tutti [[024 Macchine Decisionali|Linguaggi Decidibili]].

Si possono anche sostituire i DFA con gli [[009 Automi Finiti Non Deterministici|NFA]], data la loro [[012 Linguaggio di un NFA#Equivalenza NFA-DFA|equivalenza]].

---

### Problema dell'Accettazione di un DFA

> Sia $A$ un [[004 Automi Finiti Deterministici|DFA]] e $w$ una parola. L'automa $A$ accetta $w$?

Il corrispondente linguaggio decidibile è:
$$X_{DFA} = \{\langle A,w \rangle\ |\ A \text{ è un DFA che accetta la parola } w\}$$

### Problema del Vuoto

> Sia $A$ un DFA. Il linguaggio $\mathcal{L}(A)$ accettato da $A$ è vuoto?

Il corrispondente linguaggio decidibile è:

$$Y_{DFA} = \{\langle A \rangle\ |\ A \text{ è un DFA e } \mathcal{L}(A)=\emptyset\}$$

### Problema dell'Equivalenza

> Siano $A, B$ due DFA. I due automi sono equivalenti? Cioè sono tali che $\mathcal{L}(A) = \mathcal{L}(B)$?

$$Z_{DFA} = \{\langle A,B \rangle\ |\ A,B \text{ sono due DFA e } \mathcal{L}(A)=\mathcal{L}(B)\}$$

---

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
