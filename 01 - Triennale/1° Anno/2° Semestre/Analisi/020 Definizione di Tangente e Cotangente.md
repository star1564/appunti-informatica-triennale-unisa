---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Funzioni Trigonometriche
cssclasses:
  - 
---
Considerare un angolo $\alpha$ sulla [[018 Circonferenza Goniometrica|circonferenza goniometrica]].

![[Pasted image 20220321153828.png|400]]

## Tangente
Dato un angolo $\alpha$ sulla circonferenza goniometrica consideriamo: 
- La retta $t$, che passa solamente sul punto $S=(1,0)$ della circonferenza 
- Il punto $T$, ovvero il punto di intersezione tra la retta $t$ ed il secondo lato dell'angolo (o il suo prolungamento)

![[Pasted image 20220321161947.png|500]]

>Quindi, si dice **Tangente dell'Angolo $\alpha$** l'*ordinata del punto $T$*, ovvero
>$$\tan(\alpha)=y_T$$

In modo analogo si può definire la tangente di un angolo come il rapporto tra il [[019 Definizione di Seno e Coseno#Seno|seno]] ed il [[019 Definizione di Seno e Coseno#Coseno|coseno]].
$$\tan(\alpha)=\frac{\sin(\alpha)}{\cos(\alpha)},\quad\forall\alpha\neq \frac{\pi}{2}+k\pi, k\in\mathbb{Z}$$

La tangente *non è calcolabile* agli angoli che si ottengono sommando $\frac{\pi}{2}$ ad un certo coefficiente ($k$) moltiplicato per $\pi$. ^45b982

Ad esempio:
- $\frac{\pi}{2}$
- $\frac{3}{2}\pi$
- $\frac{5}{2}\pi$
- $\dots$

La tangente può essere:
- **Positiva**, se l'angolo $\alpha$ si trova *nel primo o nel quarto quadrante*. $$\tan(\alpha) \text{ positivo} \iff 0\leq\alpha<\frac{\pi}{2}\ \lor\ \frac{3}{2}\pi<\alpha\leq 2\pi$$
- **Negativa**, se l'angolo $\alpha$ si trova *nel secondo o nel terzo quadrante*. $$\tan(\alpha) \text{ negativo} \iff \frac{\pi}{2}<\alpha<\frac{3}{2}\pi$$


## Cotangente
Dato un angolo $\alpha$ sulla circonferenza goniometrica consideriamo: 
- La retta $c$, che passa solamente sul punto $A=(0,1)$ della circonferenza 
- Il punto $C$, ovvero il punto di intersezione tra la retta $c$ ed il secondo lato dell'angolo (o il suo prolungamento)

![[Pasted image 20220321163029.png|650]]

>Quindi, si dice **Cotangente dell'Angolo $\alpha$** l'*ascissa del punto $C$*, ovvero
>$$\cot(\alpha)=x_C$$

In modo analogo si può definire la tangente di un angolo come il rapporto tra il [[019 Definizione di Seno e Coseno#DEFINIZIONE COSENO|coseno]] ed il [[019 Definizione di Seno e Coseno#DEFINIZIONE SENO|seno]].
$$\cot(\alpha)=\frac{\cos(\alpha)}{\sin(\alpha)},\quad \forall\alpha\neq k\pi, k\in\mathbb{Z}$$

La cotangente *non è calcolabile* agli angoli che si ottengono moltiplicando un certo coefficiente ($k$) per $\pi$.  ^be5e56

Ad esempio:
- $0$
- $\pi$
- $2\pi$
- $3\pi$
- $\dots$

La cotangente può essere:
- **Positiva**, se l'angolo $\alpha$ si trova *nel primo o nel secondo quadrante*. $$\cot(\alpha) \text{ positivo} \iff 0<\alpha<\pi$$
- **Negativa**, se l'angolo $\alpha$ si trova *nel terzo o nel quarto quadrante*. $$\cot(\alpha) \text{ negativo} \iff \pi<\alpha<2\pi$$

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/le-funzioni-elementari-e-le-loro-proprieta/150-tangente-e-cotangente-di-un-angolo.html)