---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
> [!quote] <font color="orange">Teorema di Cauchy</font>
>Siano $f,g:[a,b]\to\mathbb{R}$ due [[035 Funzione Continua|funzioni continue]] su $[a,b]$ e [[046 Derivata di una Funzione#Funzione Derivabile|derivabili]] in $(a,b)$.
>Allora esiste *almeno un* punto $c\in (a,b)$, tale che 
>$$\color{#CC241D}[f(b)-f(a)]\cdot g'(c)=[g(b)-g(a)]\cdot f'(c)$$
>Dove $f(b)-f(a)$ e $g(b)-g(a)$ sono *due numeri reali*.

Si ha la seguente formula alternativa, se $g(b)\neq g(a)$ e $g'(x)\neq 0$:
$$\color{#61AFEF}\frac{f(b)-f(a)}{g(b)-g(a)}=\frac{f'(c)}{g'(c)}$$

Inoltre, se $g(x)=x$, si avrà la formula del [[054 Teorema di Lagrange|Teorema di Lagrange]].


> [!success]+ Dimostrazione
> Sia $h:[a,b]\to \mathbb{R}$ la seguente funzione: $$h(x)=[f(b)-f(a)]\cdot g(x)-[g(b)-g(a)]\cdot f(x)$$
> La funzione $h$ è <font color="#FF6611">continua</font> in $[a,b]$, poiché è la somma di funzioni continue, e <font color="#FF6611">derivabile</font> in $(a,b)$, dato che è la somma di funzioni derivabili.
> 
> Si valuta la funzione $h$ nei valori $a$ e $b$.
> 
> $$
> \begin{align*}
> h(a)&=[f(b)-f(a)]\cdot g(a)-[g(b)-g(a)]\cdot f(a) \\
> &= [(f(b)g(a))-(f(a)g(a))]-[(g(b)f(a))-(g(a)f(a))] \\
> &= f(b)g(a)-f(a)g(a)-g(b)f(a)+g(a)f(a) \\
> &= f(b)g(a)\textcolor{#61AFEF}{-f(a)g(a)}-g(b)f(a)\textcolor{#61AFEF}{+f(a)g(a)} \\
> &= \textcolor{#CC241D}{f(b)g(a)-g(b)f(a)}
> \end{align*}
> $$
> 
> 
> $$
> \begin{align*}
> h(b)&=[f(b)-f(a)]\cdot g(b)-[g(b)-g(a)]\cdot f(b) \\
> &= [(f(b)g(b))-(f(a)g(b))]-[(g(b)f(b))-(g(a)f(b))] \\
> &= f(b)g(b)-f(a)g(b)-g(b)f(b)+g(a)f(b) \\
> &= \textcolor{#61AFEF}{f(b)g(b)}-f(a)g(b)\textcolor{#61AFEF}{-f(b)g(b)}+f(a)g(b) \\
> &= -f(a)g(b)+f(a)g(b) = \textcolor{#CC241D}{f(b)g(a)-g(b)f(a)}
> \end{align*}
> $$
> 
> 
> Si ha che $\color{#FF6611}h(a)=h(b)$, quindi la funzione soddisfa le <font color="#FF6611"><i>tre ipotesi</i></font> del [[052 Teorema di Rolle|teorema di Rolle]]. Allora esiste $c\in(a,b)$ tale che: $$h'(c)=0$$
> Si calcola la derivata della funzione $h(x)$, dove si deve solamente calcolare la derivata di $g$ ed $f$, dato $f(b)-f(a)$ e $g(b)-g(a)$ sono due numeri reali. 
> $$h'(x)=[f(b)-f(a)]\cdot g'(x)-[g(b)-g(a)]\cdot f'(x)$$
> 
> Dato che esiste $c\in (a,b)$, questa derivata è zero:
> $$h'(c)=[f(b)-f(a)]\cdot g'(c)-[g(b)-g(a)]\cdot f'(c)=0$$
> 
> 
> Si porta $-[g(b)-g(a)]\cdot f'(x)$ al secondo termine e si ottiene la tesi.
> 
> $$\color{#CC241D}[f(b)-f(a)]\cdot g'(x) = f'(x)\cdot[g(b)-g(a)]$$


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
- [Francesco Bigolin](https://youtu.be/Qb8wtcC80uM?si=JGQKwZoBci0BMSVE)