---
aliases:
  - Chiusura Positiva di Kleene
  - Star
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Linguaggi
cssclasses:
  - 
---
>La **Chiusura di Kleene** (*Kleene Star* o *Star*) di un [[001 Linguaggio|linguaggio]] $L$ è il linguaggio delle stringhe ottenute *concatenando un numero qualsiasi di stringhe di $L$*, ottenuto attraverso tutte le sue [[002 Operazioni su Linguaggi#Potenza di un Linguaggio|potenze]].
>$$\textcolor{#CC241D}{L^*=\bigcup_{n\in\mathbb{N}_0}L^n}=\{w_1w_2\dots w_k\ |\ k\textcolor{#CC241D}\geq0,w_i\in L, 0\leq i\leq k\}$$
>Ovvero, *tutte le possibili stringhe che si possono comporre* a partire da ogni stringa di $L$.

Quindi, se $k=0$, la concatenazione $w_1w_2\dots w_k$ sarà la *stringa vuota* $\epsilon$.

> [!example]- <font color="orange">Esempio</font>
>- Se $L=\{a, ba\}$, allora
>$$L^0=\{\epsilon\}$$
>$$L^1=\{\textcolor{#CC241D}{a,ba}\}$$
>$$L^2=\{\textcolor{#61AFEF}{aa,aba, baa, baba}\}$$
>$$L^3=\{\textcolor{#00C575}{aaa,aaba, abaa,ababa, baaa,baaba, babaa,bababa}\}$$
>$$\dots$$
>$$L^* = \{\epsilon, \textcolor{#CC241D}{a,ba}, \textcolor{#61AFEF}{aa,aba, baa, baba}, \textcolor{#00C575}{aaa,aaba, abaa,ababa, baaa,baaba, babaa,bababa}, \dots\}=\bigcup_{n\in\mathbb{N}_0}L^n$$
>In questo caso, $bb\not\in L_1^*$.
>‎ %%← Empty Char%%
>- Se $L_2=\{a,b\}$, è come se $L_2$ fosse un alfabeto, allora
>$$L^*_2=\{\epsilon,a,b,aa,ab,ba,bb,aaa,aab,aba,\dots\}=\{a,b\}^*$$

> [!question]+ La chiusura di Kleene di un linguaggio è sempre infinita?
> No, infatti:
> - Se $L=\emptyset$, allora $L^*=\{\epsilon\}$, per via del passo base della potenza di un linguaggio
> - Se $L=\{\epsilon\}$, allora $L^*=\{\epsilon\}$ [^1]

> [!question]+ Esiste un linguaggio la cui chiusura di Kleene è l'insieme vuoto?
> No, per via della definizione del passo base della potenza di un linguaggio $(L^0 = \{\epsilon\})$, ogni chiusura di Kleene non può essere vuota.

## Chiusura Positiva di Kleene
>La **Chiusura Positiva di Kleene** per un linguaggio $L$ è la Chiusura di Kleene di $L$ dove *non si trova la parola vuota*.
>$$\textcolor{#CC241D}{L^+=\bigcup_{n\in \mathbb{N}} L^n}=\{w_1w_2\dots w_k\ |\ k\textcolor{#CC241D}>0,w_i\in L, 1\leq i\leq k\}$$

Quindi *non* si considera la potenza $L^0=\{\epsilon\}$.

> [!example]- <font color="orange">Esempio</font>
>Se $L=\{a\}$, allora $$L^+=\{a,aa,aaa,\dots\}=\{a^n\ |\ n>0, n\in \mathbb{N}\}$$

> [!question]+ Per ogni linguaggio, la sua chiusura positiva non contiene la parola vuota?
> Non per tutti i linguaggi, perché la stringa vuota si trova nella chiusura positiva di un linguaggio se $L=\{\epsilon\}$ [^1].

[^1]: ![[002 Operazioni su Linguaggi#^b36802]]

> [!question]+ Per ogni linguaggio, la sua chiusura positiva è diversa dall'insieme vuoto?
> No, perché la chiusura positiva di un linguaggio vuoto è l'insieme vuoto, in quanto non si prende la parola vuota. Quindi $$\emptyset^+=\emptyset$$



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