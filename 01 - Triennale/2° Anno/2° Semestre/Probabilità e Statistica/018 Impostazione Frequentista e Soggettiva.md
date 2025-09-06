---
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Probabilità
cssclasses:
  - 
aliases:
---
La probabilità di un [[011 Evento|Evento]] può essere definita in termini di frequenza relativa.
Supponiamo che un [[009 Esperimento|Esperimento]], il cui [[010 Spazio Campionario|Spazio Campionario]] è $S$, venga ripetuto varie volte sotto le medesime condizioni.

---
### Frequenza Assoluta
>Per ogni evento $E$ dello spazio campionario $S$, definiamo $\color{#61AFEF}n(E)$ come **Frequenza Assoluta**, ossia il numero di volte che si è verificato $E$ nelle prime $n$ ripetizioni dell'esperimento.

Notiamo che: 
- $0\leq n(E)\leq n$
- $n(S)=n$

### Impostazione Frequentista
>Nell'**Impostazione Frequentista**, la *Probabilità dell'evento $E$* è definita come $$\mathbb{P}(E)=\lim\limits_{n \to \infty}\frac{n(E)}{n}$$

Ovvero, $\mathbb{P}(E)$ è definita come [[037 Limiti di Funzioni|limite]] della *frequenza relativa* fratto $n$, ossia limite della proporzione del numero di volte che l'evento $E$ si verifica.

Ovvero, al crescere del numero delle prove fatte tutte nelle stesse condizioni, la frequenza relativa, pur variando, tende a *stabilizzarsi attorno ad un valore* che corrisponde al valore della probabilità dell'evento.


> [!example]- <font color="orange">Esempio</font>
>Se in $n=10$ lanci di una moneta esce 6 volte testa, allora $n(E)=6$, con $E=\{t\}$ e $S=\{c,t\}$.
>Quindi si ha la probabilità $\mathbb{P}(E)=\frac{6}{10}=0.6=60\%$
>
>![[Pasted image 20240228131909.png|600]]

### Impostazione Soggettiva
Secondo l'*Impostazione Soggettiva*, la probabilità di un evento è il grado di fiducia che un individuo ha nel verificarsi dell'evento.

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