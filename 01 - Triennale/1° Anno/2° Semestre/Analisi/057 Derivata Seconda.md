---
aliases:
  - Punto di flesso
  - Punto di Flesso Discendente
  - Punto di Flesso Ascendente
  - Punto di Flesso a Tangente Orizzontale
tags:
  - corsi/matematica/analisi
paragrafo: Derivate
cssclasses:
  - 
---
>La **Derivata Seconda** è essenzialmente la *derivata della [[046 Derivata di una Funzione#Derivata Prima|derivata prima]]*. Si denota con $$\color{#CC241D}f''(x)$$

Si può notare che:
1. La funzione $f(x)$ è [[056 Funzioni Convesse e Concave#Funzione Convessa|convessa]] in $(a,b)$ se e solo se: $$f''(x)\geq0,\quad \forall x\in(a,b)$$
2. La funzione $f(x)$ è [[056 Funzioni Convesse e Concave#Funzione Concava|concava]] in $(a,b)$ se e solo se: $$f''(x)\leq0,\quad \forall x\in(a,b)$$

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20241004190641.png|700]]
>- $f(x)=\sin(x)$ è in verde
>- $f'(x)=\cos(x)$ è in blu
>- $f''(x)=-\sin(x)$ è in rosso tratteggiato

### Punti Di Flesso
>Sulle funzione si presentano molto spesso uno o più *Cambi di Concavità*. Un cambio di concavità fa sì che una data funzione *diventi da concava a convessa o viceversa*.
>
>Il punto in cui avviene questo cambio prende il nome di **Punto di Flesso**.

Sia $f$ derivabile due volte in un intorno di $x_0$. Valgono le seguenti implicazioni:
- Se la derivata prima $\color{#61AFEF}f'(x_0)=0$, allora il punto $x_0$ è un **Punto di Flesso a Tangente Orizzontale**
  
  ![[Pasted image 20220505111339.png|500]]
 ^39c23f
- Se $f''(x)$ è *negativa in un intorno sinistro* di $x_0$ ed è *positiva in un intorno destro* di $x_0$, allora $x_0$ è un **Punto di Flesso Ascendente**

- Se $f''(x)$ è *positiva in un intorno sinistro* di $x_0$ ed è *negativa in un intorno destro* di $x_0$, allora $x_0$ è un **Punto di Flesso Discendente**

> [!example]+ <font color="orange">Esempi</font>
>![[Pasted image 20241004180012.png]]
>
>---
>![[Pasted image 20241004193236.png|900]]
>- $f(x)=\sin(x)$ è in verde
>- $f'(x)=\cos(x)$ è in blu
>- $f''(x)=-\sin(x)$ è in rosso tratteggiato


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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/derivate/2297-teoremi-derivata-seconda.html)