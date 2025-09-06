---
aliases:
  - Funzione Integrale
  - Secondo Teorema Fondamentale del Calcolo Integrale
  - Torricelli-Barrow
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
>Il **Teorema Fondamentale del Calcolo Integrale** è un teorema che *stabilisce la continuità della funzione integrale*, e sotto opportune ipotesi, anche *la sua [[046 Derivata di una Funzione#Funzione Derivabile|derivabilità]]*.

Il teorema si suddivide in due parti, dette:
- *Primo teorema fondamentale del calcolo integrale*, che fornisce i risultati qualitativi relativi alla funzione integrale;
- *Formula Fondamentale del Calcolo Integrale*, anche chiamato **Teorema di Torricelli-Barrow**, fornisce una formula che permette di calcolare esplicitamente gli [[066 Integrali#Funzione Qualsiasi e Integrale Definito|integrali definiti]] utilizzando il concetto di [[067 Primitiva di una Funzione|primitiva di una funzione]].

> [!quote]+ Funzione Integrale
>Considerare una funzione $f:[a,b]\to\mathbb{R}$, limitata e integrabile secondo Riemann in $[a,b]$. 
>
>La funzione $G$ viene detta **Funzione Integrale** di $f$ su $[a,b]$ se, per ogni $x\in[a,b]$, si pone che: $$\color{#61AFEF}G(x)=\int_a^x f(t)\,dt$$


## Primo Teorema Fondamentale
>Sia $f:[a,b]\to\mathbb{R}$ una funzione limitata e integrabile in $[a,b]$. 
>Allora la funzione integrale $G(x)$ è [[035 Funzione Continua|continua]] nell'intervallo $[a,b]$.
>
>Se inoltre $f(x)$ è una funzione continua su $(a,b)$, allora la funzione integrale $G(x)$ è [[046 Derivata di una Funzione#Funzione Derivabile|derivabile]] in ogni punto in cui $f(x)$ è continua, e risulta che $$G'(x)=f(x_0)$$

## Secondo Teorema Fondamentale (Torricelli-Barrow)
>Sia $f:[a,b]\to\mathbb{R}$ una funzione che ammette una primitiva $F(x)$ su $[a,b]$. 
>Allora vale la **formula fondamentale del calcolo integrale**: 
>$$\int_a^b f(x)dx=\textcolor{#CC241D}{[F(x)]_a^b}=\color{#61AFEF}F(b)-F(a)$$

Essenzialmente, per calcolare un [[066 Integrali#Funzione Qualsiasi e Integrale Definito|integrale definito]], si deve:
1. Trovare l'[[068 Integrali Indefiniti|integrale indefinito]] della funzione
2. Calcolare la [[067 Primitiva di una Funzione|primitiva]] dell'integrale indefinito
	- Per individuare la primitiva si possono usare le [[069 Primitive Fondamentali|Primitive Elementari]]
3. Calcolare la primitiva nei due *estremi di integrazione* $a$ e $b$, ovvero si calcola $F(b)$ e $F(a)$
4. Fare la *differenza* dei due numeri ottenuti (le costanti di integrazione $c$ *si annullano* nel calcolo), ovvero $$\int_a^b f(x)\,dx=\textcolor{#CC241D}{[F(x)]_a^b}=\color{#61AFEF}F(b)-F(a)$$

> [!example]+ <font color="orange">Esempio</font>
>$$\int_0^5 3x^2\,dx$$
>
>L'integrale indefinito è $$\int 3x^2\,dx= 3\int x^2\,dx = 3\cdot \frac{x^{2+1}}{2+1}= \frac{3x^3}{3} = x^3$$
>
>Si sono usate le [[070 Proprietà degli Integrali|Proprietà degli Integrali]] e la [[069 Primitive Fondamentali#^34736c|primitiva fondamentale di x elevato alla n]].
>
>Infatti, si ha che $$\frac{d}{dx}\left[ x^3 \right]\,dx=3x^2$$
>
>Pertanto, si ha $$\int_0^5 3x^2\,dx=\left[x^3\right]_0^5=(5)^3-(0)^3=125$$


# %%FinePagina%%

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/integrali/653-teorema-fondamentale-del-calcolo-integrale.html)