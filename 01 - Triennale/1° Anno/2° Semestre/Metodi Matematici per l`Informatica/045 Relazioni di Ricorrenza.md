---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Dimostrazioni per induzione
cssclasses:
  - 
---
>Una **Relazione di Ricorrenza** per la [[029 Successioni|successione]] $(a_n)_{n\in\mathbb{N}}$ è un'equazione che *esprime $a_n$ in funzione di uno o più dei termini precedenti $a_0, ..., a_{n-1}$* della successone, per ogni intero positivo $n$, con $n\geq n_0$.

Affinché la relazione di ricorrenza definisca univocamente la sequenza devono essere definite le *condizioni iniziali*.

Queste definiscono i termini che precedono il primo termine a cui la relazione di ricorrenza si applica.

> [!example]+ <font color="orange">Esempio</font>
> > Trovare la relazione di ricorrenza, con la condizione iniziale, che definisce la successione $(f_n)_{n\in\mathbb{N}}$ dei numeri di Fibonacci.
> 
> $f_0=1, f_1=1$ (*Condizioni Iniziali*)
> $f_n=f_{n-1}+f_{n-2},\quad n\geq2$ (*Relazione di Ricorrenza*)

Quindi dobbiamo partire da una definizione ricorsiva di una successione e vogliamo trovare una sua definizione "differente".

*Risolvere una relazione di ricorrenza*, con le condizioni iniziali, significa trovare una formula esplicita, chiamata **Soluzione in Forma Chiusa**, per i termini $a_n$ della sequenza.

Si vuole cioè trovare una definizione dei termini $a_n$ della sequenza in cui scompaiono le dipendenze di $a_n$ dai termini precedenti della sequenza.

## Metodi di risoluzione
Esistono alcuni metodi per risolvere una relazione di ricorrenza, tra cui il **Metodo di Iterazione**.

L'idea è quella di applicare (iterare) la ricorrenza ed esprimerla come somma di termini dipendenti solo da $n$ e dalle condizioni iniziali.

> [!example]- <font color="orange">Esempio</font>
> > $T(1)=2$
> > $T(n)=T(n-1)+3,\quad n>1$
> 
> Partiamo dalla relazione.
> Sostituiamo a $T(n-1)$ l'espressione della relazione su $n-1$.
> - $T(n)=T(n-1)+3$
> - $T(n)=[T(n-2)+3]+3=T(n-2)+3\cdot2$
>
> Sostituiamo a $T(n-2)$ l'espressione della relazione su $n-2$.
> - $T(n)=T(n-1)+3$
> - $T(n)=[T(n-2)+3]+3=T(n-2)+3\cdot2$
> - $T(n)=[T(n-3)+3]+3\cdot2=T(n-3)+3\cdot3$
> - $...$
> - $T(n)=T(n-i)+3\cdot i,\quad 1\leq i\leq n-1$
>
> Finiremo quando $n-1=i$, quindi $i=n-1$.
> $\color{#61AFEF}T(n)=T(1)+3(n-1)=2+3(n-1)=3n-1$
> Questa è la forma chiusa.
>
> Si può usare il [[046 Principio di Induzione Matematica|principio di induzione matematica]] per provare che la soluzione è corretta.
> **Passo Base**: $T(1)=3\cdot1-1=2$.
> **Passo Induttivo**: Se $a_{n-1}=3(n-1)-1=T(n-1)$, allora  $T(n)=T(n-1)+3=3(n-1)-1+3=3n-1$.




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


