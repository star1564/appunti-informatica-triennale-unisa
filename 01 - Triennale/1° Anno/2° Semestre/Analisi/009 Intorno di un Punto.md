---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Nozioni di base
cssclasses:
  - 
---
>L'**Intorno** di un punto $x_0\in\mathbb{R}$ è un [[004 Intervalli#Intervallo Aperto|intervallo aperto a sinistra e a destra]] *centrato nel punto $x_0$*. Questo intervallo si espande sia a sinistra sia a destra di tale punto, con lunghezza arbitraria.

Il punto di riferimento $x_0$ viene detto *centro dell'intorno*, l'ampiezza dell'intervallo viene detta *raggio dell'intorno*.

L'intorno *completo* del punto $x_0$ di raggio $\epsilon$, si definisce come l'intervallo aperto sia a sinistra che a destra della forma:
$$B(x_0,\epsilon)=\textcolor{#61AFEF}{(x_0-\epsilon, x_0+\epsilon)} =\{x\in\mathbb{R}:\ x_0-\epsilon<x<x_0+\epsilon\}$$

![[Immagine 2022-03-10 145854.png|450]]

Viene anche indicato solamente specificando il punto centrale:
$$B_{x_0}$$



## Intorno Sinistro e Destro
### Intorno Sinistro
>Si definisce l'**intorno sinistro** di un punto $x_0$ di raggio $\epsilon$ l'intervallo aperto a sinistra e a destra dato da
>$$B^{\ -}(x_0,\epsilon)=\textcolor{#61AFEF}{(x_0-\epsilon, x_0)}=\{x\in\mathbb{R}:\ x_0-\epsilon<x<x_0\}$$

![[Pasted image 20220310150450.png|400]]

### Intorno Destro
>Si definisce l'**intorno destro** di un punto $x_0$ di raggio $\epsilon$ l'intervallo a sinistra e a destra aperto dato da
$$B^{\ +}(x_0,\epsilon)=\textcolor{#61AFEF}{(x_0, x_0+\epsilon)}=\{x\in\mathbb{R}:\ x_0<x<x_0+\epsilon\}$$

![[Pasted image 20220310150633.png|400]]

## Intorno di Infinito
### Intorno di $-\infty$
>Si definisce **intorno di meno infinito** un qualsiasi intervallo aperto a sinistra e a destra del tipo
>$$(-\infty, M)=\{x\in\mathbb{R}: x<M\}$$
>dove $M$ è un valore reale.

![[Pasted image 20220310150907.png|400]]

### Intorno di $+\infty$
>Si definisce **intorno di più infinito** un qualsiasi intervallo aperto a sinistra e a destra del tipo
>$$(M, +\infty)=\{x\in\mathbb{R}: x>M\}$$
>dove $M$ è un valore reale.

![[Pasted image 20220310151019.png|400]]

## Intorno Bucato
>Si definisce l'**intorno bucato** di un punto $x_0$ un intorno $B(x_0, \epsilon)$ *di cui è privato del punto $x_0$*, ovvero:
$$B^{\ *}(x_0, \epsilon) = B(x_0, \epsilon)-\{x_o\}=(x_0-\epsilon, x_0)\ \cup\ (x_0, x_0+\epsilon)$$

![[Pasted image 20220310151333.png|400]]

## Punto di Accumulazione
>Un punto $x_0\in\mathbb{R}$ è detto **Punto di Accumulazione** per un insieme $E\subseteq \mathbb{R}$ se, comunque scelto un intorno completo $B(x_0,\epsilon)$, risulta che $B(x_0,\epsilon)$ contiene almeno un punto di $E$ diverso da $x_0$.
>$$\forall\epsilon>0,\ \exists y\in E,\ y\neq x_0:\ y\in B(x_0, \epsilon)$$


> [!example]- <font color="orange">Esempio</font>
>Comunque consideriamo un intorno di un fissato punto $x_0$ contenuto nell'intervallo (in nero), troviamo sempre almeno un punto $c$ dell'intervallo tale da appartenere all'intorno e che sia diverso da $x_0$ stesso.
>
>![[Pasted image 20220401115754.png|700]]
> 
> ---
>Invece un punto non è di accumulazione per un insieme (in nero) se abbiamo un caso del genere.
>
>![[Pasted image 20220401115922.png|700]]
>
>Anche se c'è un intorno (arancione) che contiene almeno un punto dell'intervallo nero che sia diverso da $x_0$, si dovrebbe avere che questo sia vero per ogni intorno (come quello rosso).

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/premesse-per-lanalisi-infinitesimale/55-la-nozione-di-intorno-di-un-punto.html)