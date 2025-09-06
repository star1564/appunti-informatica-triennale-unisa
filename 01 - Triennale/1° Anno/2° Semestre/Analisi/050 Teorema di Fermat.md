---
aliases:
  - Punto Stazionario
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
## Punto Stazionario
>Un **Punto Stazionario** è un punto $x$ interno al dominio della funzione $f$ che *annulla la sua derivata*, ovvero se:
>1. La funzione $f$ è [[046 Derivata di una Funzione#Funzione Derivabile|derivabile]] in $x$
>2. La [[046 Derivata di una Funzione#Derivata Prima|derivata prima]] di $f$ è zero in $x$ $$f'(x)=0$$

Infatti, un punto è stazionario, quando la retta tangente in quel punto *è parallela all'asse delle ascisse*.

![[Pasted image 20220422161235.png|650]]

> [!example]- <font color="orange">Esempio</font>
>$$f(x)=x^2,\quad f'(x)=2x$$
>
>![[Pasted image 20241003145742.png|380]]
>
>$x=0$ è l'unico punto stazionario della funzione.

## Teorema di Fermat

> [!quote] <font color="orange">Teorema di Fermat </font>
>Il **Teorema di Fermat** stabilisce che, in una funzione $f:(a,b)\to \mathbb{R}$ derivabile, se un punto $x_0\in (a,b)$ è un punto di [[049 Massimi e Minimi Relativi e Assoluti#Massimo Relativo (o Locale)|massimo locale]] (o [[049 Massimi e Minimi Relativi e Assoluti#Minimo Relativo (o Locale)|minimo locale]]) per $f$, allora $x_0$ è un [[#Punto Stazionario|Punto Stazionario]].

![[Pasted image 20241003150806.png|500]]

> [!success]+ Dimostrazione (pL 194, Aracne)
>Si supponga che $x_0\in (a,b)$ sia un punto di massimo relativo. Allora esiste un [[009 Intorno di un Punto|intorno]] $I_{x_0}$ con centro $x_0$ tale che
>$$f(x)-f(x_0)\leq 0, \quad \forall x\in I_{x_0}$$
>Infatti, se $x_0$ è un punto di massimo, spostandosi sull'asse delle ascisse (sia a destra che a sinistra) si noterà che valori della funzione sono *più piccoli di $f(x_0)$*.
>
>Quindi, considerato $x_0+h\in I_{x_0}$, con $h\in \mathbb{R}$, si ha che:
>$$f(x_0+h)\leq f(x_0)$$
>
>Pertanto, si ottiene il [[045 Rapporto Incrementale|Rapporto Incrementale]] e lo si mette in due [[MB - 02 Disequazioni e Disuguaglianze#Disuguaglianza|disuguaglianze]]:
>$$\frac{f(x_0+h)-f(x_0)}{h}\leq 0,\quad h>0$$
>$$\frac{f(x_0+h)-f(x_0)}{h}\geq 0,\quad h>0$$
>
>Si hanno due casi, dato che $f(x_0)>f(x_0+h)$:
>1. Si ottiene la frazione $\frac{-}{+}=-$
>2. Si ottiene la frazione $\frac{+}{+}=+$
>
>
>Siccome la funzione è derivabile in $x_0$, con il teorema della permanenza del segno, si passa al limite per $h\to 0$ in entrambe le disuguaglianze, ottenendo:
>$$f_+'(x_0)=\lim\limits_{h \to 0^+}\frac{f(x_0+h)-f(x_0)}{h}\leq 0$$
>$$f_-'(x_0)=\lim\limits_{h \to 0^-}\frac{f(x_0+h)-f(x_0)}{h}\geq 0$$
>Questi sono rispettivamente il limite destro e sinistro della derivata prima.
>
>Dato che
>$$f'_+(x_0)\leq0\ \land\ f'_-(x_0)\geq0$$
>l'unico caso possibile è $$f'_+(x_0)=0=f'_-(x_0)$$
>ovvero $$f'(x_0)=0$$

___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/derivate/400-teorema-di-fermat.html)
- [Francesco Bigolin](https://www.youtube.com/watch?v=AZnaVU1xdFg)