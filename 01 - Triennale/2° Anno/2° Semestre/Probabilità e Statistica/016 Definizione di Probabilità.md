---
aliases:
  - Probabilità semplice
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Probabilità
cssclasses:
  - 
---
>Dato uno [[010 Spazio Campionario|Spazio Campionario]] $S$, la **Probabilità** $\color{#CC241D}\mathbb{P}$ è una [[011 Funzioni|funzione]] che prende un [[011 Evento|Evento]] $E\subseteq S$ come input e restituisce come output un valore tra $0$ e $1$. 
>$$\mathbb{P}(E)\in [0, 1]$$

## Definizione classica di Probabilità di un Evento
>La probabilità di un evento è data dal rapporto tra il numero di *casi favorevoli per il realizzarsi dell'evento* ed il numero di *casi possibili*.
>$$\textcolor{#CC241D}{\mathbb{P}(E)}=\frac{\text{\# casi favorevoli}}{\text{\#\ casi possibili}}=\frac{|E|}{|S|}$$

Per ottenere la *percentuale* di probabilità, si esegue $(P(A)\cdot 100)\%$.

> [!note] Spazi campionari con esiti equiprobabili
> Quando si specifica che l'avvenirsi di un esperimento non è truccato, *si assume che tutti i risultati siano equiprobabili* e che lo *spazio campionario sia finito*.
> $$\mathbb{P}(E_1)=\mathbb{P}(E_2)=\ldots=\mathbb{P}(E_n)$$

> [!example]- <font color="orange">Esempi</font>
> Il lancio di una moneta *non truccata* ha quattro esiti: $S=\{tt, tc, ct, cc\}$.
> La probabilità che si verifichi l'evento che esca solo $\{cc\}$ è: $$P(E)=\frac{|E|}{|S|}=\frac{1}{4}$$
> 
> ---
> >Se estraiamo 3 biglie a caso da un'urna che contiene 6 biglie bianche e 5 nere (11 in totale), qual è la probabilità che una sia bianca e le altre due nere?
>
>
>Se non teniamo conto dell'ordine di estrazione e se non si fa reinserimento, la probabilità richiesta è espressa mediante numeri di combinazioni:
>$$\frac{{6\choose 1}{5\choose 2}}{{11\choose 3}}=\frac{6\cdot10}{165}=\frac{4}{11}=0,3636$$


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
- [Michel van Biezen](https://www.youtube.com/watch?v=WelPq6Kgf7s&list=PLX2gX-ftPVXUWwTzAkOhBdhplvz0fByqV&index=8)
- [YouMath](https://www.youmath.it/lezioni/probabilita/probabilita-discreta/1199-calcolare-probabilita-di-un-evento.html)
- https://www.youtube.com/watch?v=ynjHKBCiGXY&list=PL0KQuRyPJoe6KjlUM6iNYgt8d0DwI-IGR&index=21