---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Calcolo Combinatorio
cssclasses:
  - 
---
>Una **Disposizione** è una *[[026 Sequenza|sequenza ordinata]]* di $k$ oggetti **estratti** da un insieme che ne contiene $n$ distinti. Quindi, [[Considerare l'ordine in Combinatoria|l'ordine viene considerato]].
>
>Si ha che:
>- In ogni sequenza ci sono esattamente $k$ elementi
>- Ciascuna sequenza deve differire dalle altre *per almeno un elemento* oppure *per l'ordine* con cui gli elementi si susseguono

![[Pasted image 20230930145803.png|400]]

> [!example]+ <font color="orange">Esempio</font>
>Consideriamo l'insieme $A=\{a,b,c\}$ formato da $n=3$ elementi, e sia $k=2$.
>
>Le disposizioni di $A$ sono tutti i possibili raggruppamenti con 2 lettere dell'insieme e che si differenziano tra loro per almeno un elemento oppure per l'ordine con cui gli elementi si presentano:
>$$aa,ab,ac$$ $$ba,bb,bc$$ $$ca,cb,cc$$


## Tipi di Disposizioni
Ci sono due tipi di disposizioni.

### Disposizioni Semplici
>Una **disposizione semplice** è una sequenza ordinata di *$k$ elementi distinti* (dove *non* sono ammesse ripetizioni) estratti tra $n$ elementi distinti, con $n\ge k$.
>
>Il numero di disposizioni semplici si indica con $D_{n,k}$ ed è dato dal [[004 Fattoriale Discendente|Fattoriale Discendente]].
>$$D_{n,k}=(n)_{k}=\color{#CC241D}\frac{n!}{(n-k)!}$$

![[Pasted image 20240801164619.png|500]]


> [!example]- <font color="orange">Esempio</font>
>Consideriamo l'insieme $A=\{a,b,c\}$ formato da $n=3$ elementi, e sia $k=2$.
>
>Abbiamo che il numero delle disposizioni semplici è 
>$$(n)_k=(3)_2=\frac{3!}{(3-2)!}=3! =6$$
>
>Le disposizioni semplici di $A$ sono le seguenti:
>$$ab,ac$$ $$ba,bc$$ $$ca,cb$$
>
>---
> Supponiamo di voler prendere gli elementi $\{1,2,3\}$ e creare delle sequenze di 2 elementi. Quindi $n=3, k=2$.
> 
> ![[Pasted image 20230926182659.png|700]]
> Da notare che il numero di rami nel grafo per ogni livello (dove il livello radice non viene considerato, 1° livello = {1}, {2}, {3}) si riduce di 1.

### Disposizioni con Ripetizione
>Una **disposizione con ripetizione** è una sequenza ordinata di $k$ elementi estratti tra $n$ elementi distinti, con la possibilità che ogni elemento possa *ripetersi fino a $k$ volte* nella stessa sequenza.
>
>Il numero di disposizioni con ripetizione si indica con $D'_{n,k}$ ed è dato dalla potenza $k$-esima di $n$.
>$$D'_{n,k}=\textcolor{#CC241D}{n^k}$$

![[Pasted image 20240801164737.png|500]]

> [!example]- <font color="orange">Esempio</font>
>Consideriamo l'insieme $A=\{a,b,c\}$ formato da $n=3$ elementi, e sia $k=2$.
>
>Abbiamo che il numero delle disposizioni con ripetizione è 
>$$n^k = 3^2=9$$
>
>Le disposizioni semplici di $A$ sono le seguenti:
>$$aa,ab,ac$$ $$ba,bb,bc$$ $$ca,cb,cc$$
>
>---
> Supponiamo di voler prendere gli elementi $\{1,2,3\}$ e creare delle sequenze di 2 elementi. Quindi $n=3, k=2$.
> 
> ![[Pasted image 20230926183138.png|700]]
> Da notare che il numero di rami nel grafo per ogni livello (dove il livello radice non viene considerato, 1° livello = {1}, {2}, {3}) rimane lo stesso ($n$).

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
- [YouMath - Disposizioni](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/5158-disposizione-matematica.html)
- [YouMath - D. Semplici](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/1214-disposizione-semplice.html)
- [YouMath - D. Ripetizione](https://www.youmath.it/lezioni/probabilita/calcolo-combinatorio/1215-disposizioni-con-ripetizione.html)