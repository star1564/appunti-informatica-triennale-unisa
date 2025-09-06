---
aliases:
  - Algoritmo di Euclide
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
>L'**algoritmo delle divisioni successive** (algoritmo di Euclide) è un metodo utilizzato per *eseguire la divisione con resto* tra due numeri interi. 
>È utile per calcolare il [[045 Massimo Comun Divisore|MCD]].

È un *processo iterativo* che scompone la divisione in una serie di passaggi più piccoli, facilitando la comprensione e l'esecuzione della divisione.

Si può scrivere come: $$r=rest(a,b)$$

> [!quote] Definizione Formale
> Siano $a,b\in\mathbb{N}$, con $a\geq b$ e $b\ne 0$. Esistono e sono unici due numeri naturali q, detto *quoziente*, ed r, detto *resto*, tali che:
>$$a=b\times q+r,\quad\quad 0\le r < |b|$$

> [!summary] Procedimento
>1. Inizia con due numeri interi, $a$ e $b$, dove $a$ è maggiore o uguale a $b$.
>2. Effettua la divisione intera di $a$ per $b$ per ottenere il quoziente $q$ e il resto $r$.
>3. Sostituisci $a$ con $b$ e $b$ con $r$, quindi torna al passo 2 e ripeti il processo. Continua a eseguire la divisione finché il resto $r$ non diventa zero.

> [!example]- <font color="orange">Esempio</font>
>$a=512, b=2$.
>$512 = 256\times2 + 0\quad$, $\quad$ovvero $512/2 = 256$ con resto $0$
>$256 = 128\times2 + 0\quad$, $\quad$...
>$128 = 64\times2 + 0$
>$64 = 32\times2 + 0$
>$32 = 16\times2 + 0$
>$16 = 8\times2 + 0$
>$8 = 4\times2 + 0$
>$4 = 2\times2 + 0$
>$2 = 1\times2 + 0$
>$1 = 0\times2 + 1$

> [!info]- Teorema di [[027 Ricorsione Matematica|ricorsione]] del MCD
> Per qualsiasi numero intero $a$ non negativo e qualunque intero $b$ positivo, si può utilizzare il [[050 Congruenza Modulo|Modulo]] $$MCD(a,b)=MCD(b,a(\text{mod }  b))$$
> 
>```C
>int MCD(a,b) {
>	if (b == 0)
>		return a;
>	else
>		return MCD(a, mod(a, b));
>}
>```


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