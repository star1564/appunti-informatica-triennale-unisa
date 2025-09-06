---
aliases: 
tags:
  - corsi/matematica/ricerca_operativa
paragrafo: Vettori
cssclasses:
  - 
---
> [!error] !!! Le note di Ricerca Operativa non sono complete e in parte sono errate !!!

## Somma e Differenza
>Due vettori della *stessa dimensione* possono essere **sommati**, dove l'addizione è effettuata *componente per componente*. Quindi se abbiamo due vettori $\underline{x} ,\underline{w}$, abbiamo che 
>$$\textcolor{#CC241D}{\underline{x}+\underline{w}}=\begin{bmatrix} x_1+w_1 \\ x_2+w_2 \\ \dots \\ x_n+w_n  \end{bmatrix}$$

La situazione è analoga per la **differenza**, dove si sostituisce $+$ con $-$.

> [!example]- <font color="orange">Esempio</font>
>$$\underline{x}=\begin{bmatrix} 1 \\ 4  \end{bmatrix}, \underline{w}=\begin{bmatrix} 2 \\ 2  \end{bmatrix}$$
>
>$$\underline{x}+\underline{w}=\begin{bmatrix} 1+2 \\ 4+2  \end{bmatrix}=\begin{bmatrix} 3 \\ 6  \end{bmatrix}$$
>$$\underline{x}-\underline{w}=\begin{bmatrix} 1-2 \\ 4-2  \end{bmatrix}=\begin{bmatrix} -1 \\ 2  \end{bmatrix}$$

### Regola del Parallelogramma
Questa regola è un *metodo grafico* con cui è possibile individuare il **vettore somma associato** a due vettori:
- Si posizionano entrambi i vettori su un asse cartesiano, con entrambi i punti di partenza sull'origine $[0\ 0]$ dell'asse.
- Si trasla il primo vettore ($\underline{x}$) sul secondo vettore ($\underline{w}$) e si traccia una linea tratteggiata.
- Si trasla il secondo vettore ($\underline{w}$) sul primo vettore ($\underline{x}$) e si traccia una linea tratteggiata.
- Si traccia un nuovo vettore che parte dall'origine dell'asse fino all'intersezione delle due linee tratteggiate. Questo è il vettore somma.

![[Pasted image 20240323165335.png|500]]

## Moltiplicazione di un Vettore per uno Scalare

>L'operazione di **moltiplicazione di un vettore $\underline{a}$ con uno [[002 Vettori#Scalare|scalare]] $k$** è effettuato *componente per componente*.

- Se $k>0$, allora $k\underline{a}$ punta nella *stessa direzione* di $a$
- Invece, se $k<0$, allora $k\underline{a}$ punta nella *direzione opposta* di $a$
- Se $k=0$, allora $k\underline{a}$ è il [[002 Vettori#Vettore Nullo|Vettore Nullo]]

> [!example]- <font color="orange">Esempio</font>
>Dato il vettore
$$\underline{x}=\begin{bmatrix} -1 \\ 2  \end{bmatrix}$$
e lo scalare $2$, supponiamo di voler eseguire la moltiplicazione del vettore con lo scalare $$2\underline{x}=\begin{bmatrix} -1 \\ 2  \end{bmatrix}=2\begin{bmatrix} -1 \\ 2  \end{bmatrix}=\begin{bmatrix} -2 \\ 4  \end{bmatrix}$$
>
>![[Pasted image 20240323170256.png|600]]

## Prodotto Interno
>Due vettori con lo stesso numero $n$ di componenti possono essere *moltiplicati tra di loro*. Il risultato di questa moltiplicazione è un numero reale chiamato **prodotto interno** dei due vettori. È definita come segue, dove $\underline{a}, \underline{b}$ sono vettori $$\underline{ab}=a_1b_1+a_2b_2+\dots+a_nb_n=\displaystyle\sum_{i=1}^{n} a_ib_i$$


> [!example]- <font color="orange">Esempio</font>
>$$\underline{x}^T=[0 \ 2], \underline{w}=\begin{bmatrix} 3 \\ 4  \end{bmatrix}$$
>
>$$\underline{x^Tw}=0\cdot3+2\cdot4=0+8=8$$



___
[[000 Indice RO|↖ Ritorna all'indice ↖]]

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
- [YouMath](https://www.youmath.it/lezioni/algebra-lineare/matrici-e-vettori/694-somma-di-vettori-prodotto-per-uno-scalare-regola-del-parallelogramma.html)