---
aliases:
  - Concatenazione di linguaggi
  - Prodotto di linguaggi
  - Potenza di linguaggi
  - Reverse di linguaggi
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Linguaggi
cssclasses:
  - 
---
Dato che i [[001 Linguaggio|linguaggi]] sono degli [[001 Insieme|Insiemi]], sono disponibili le operazioni di: 
- [[008 Unione tra Insiemi|Unione]]
- [[009 Intersezione tra Insiemi|Intersezione]]
- [[010 Differenza tra Insiemi|Differenza]]
- [[013 Complemento di un Insieme|Complemento]]

## Concatenazione (o prodotto) di Linguaggi
>Dati due linguaggi $S$ e $T$ sull'alfabeto $\Sigma$, il **Prodotto** (o *concatenazione*) di $S$ e $T$ è $$ST=S\circ T=\{uv\in\Sigma^*\ |\ u\in S,v\in T\}$$

> [!note]+ Non è il prodotto cartesiano
> Il prodotto di linguaggi è un'operazione **diversa** dal [[012 Prodotto Cartesiano tra Insiemi|prodotto cartesiano]], infatti, se $S=\{a,ba,bb\}$ e $T=\{\epsilon,ba\}$ allora 
> $$ST=\{a,aba,ba,baba,bb,bbba\}$$
> $$S\times T =\{(a,\epsilon),(a,ba),(ba,\epsilon), (ba,ba), (bb,\epsilon), (bb,ba)\}$$
> $$\color{#CC241D}ST\neq S\times T$$

Inoltre, se $S$ e $T$ sono linguaggi finiti: 
- $|ST|\leq|S||T|$, perché potrebbero esserci delle ripetizioni (che non vengono contate) in $ST$ 
- $ST\neq TS$
- $\color{#61AFEF}\emptyset \circ L = L \circ \emptyset = \emptyset$, ovvero che l'insieme vuoto azzera
- $\color{#61AFEF}\{\epsilon\} \circ L = L \circ \{\epsilon\} = L$, ovvero che la stringa vuota è l'elemento neutro

> [!example]- <font color="orange">Esempio</font>
>Se $S=\{a,aa\}$ e $T=\{\epsilon,a,ba\}$, allora $$ST=\{a,aa,aba,aaa,aaba\},\quad TS=\{a,aa,aaa,baa,baaa\}$$
>Infatti $aba\in ST$, ma $aba\not\in TS$. Quindi $ST\neq TS$.

## Potenza di un Linguaggio
Sia $L$ un linguaggio sull'alfabeto $\Sigma$.

>La **Potenza** di un linguaggio si definisce come la *concatenazione delle sue stringhe per le stringhe della potenza precedente*:
>- **Passo Base**: 
>$$\color{#61AFEF}L^0=\{\epsilon\}$$
>- **Passo Ricorsivo**: 
>$$\color{#61AFEF}L^k=L^{k-1}L,\ k\geq1$$
>Ovvero $L^k=\{w_1w_2\dots w_k\ |\ w_i\in L, 1\leq i\leq k, k\geq 0\}$

^891881

Inoltre si ha che: 
- $\emptyset^0=\{\epsilon\}$ per via del passo base
- $\emptyset^n=\emptyset, n\in \mathbb{N}$ per via della concatenazione con il linguaggio vuoto
- $\{\epsilon\}^n=\{\epsilon\}, n\in\mathbb{N}$ per via della concatenazione del linguaggio della stringa vuota ^b36802

> [!example]- <font color="orange">Esempio</font>
>Se $L=\{a,bb\}$, allora:
>- $L^0=\{\epsilon\}$
>- $L^1=\{a,bb\}$
>- $L^2=\{aa,abb,bba,bbbb\}$
>- $L^3=\{aaa,aabb,abba,abbbb, bbaa,bbabb,bbbba,bbbbbb\}$

## Reverse di un Linguaggio

>Sia $L$ un linguaggio sull'alfabeto $\Sigma$.
>Il **Reverse** di un linguaggio è lo stesso linguaggio dove è stata effettuata l'operazione di [[036 Inverso di una Stringa|reverse su ogni sua stringa]].
>$$L^R=\{w\in \Sigma^*\ |\ \exists u \in L:w=u^R\}$$

___
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