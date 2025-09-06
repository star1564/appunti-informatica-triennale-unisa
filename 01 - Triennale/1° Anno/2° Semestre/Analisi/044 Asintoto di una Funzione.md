---
aliases:
  - Asintoto Verticale
  - Asintoto Orizzontale
  - Asintoto Obliquo
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Funzioni
cssclasses:
  - 
---

## Asintoto Verticale
>Un **Asintoto Verticale** è una *retta verticale* che approssima l'andamento del grafico di una funzione nell'[[009 Intorno di un Punto|intorno]] di un punto $x_0$ *non definito* nella funzione.

Considerare:
- $f:Dom(f)\to\mathbb{R}$ 
- $x_0$, un [[009 Intorno di un Punto#Punto di Accumulazione|punto di accumulazione]] del dominio di $f(x)$

Si dice che la retta verticale $\color{#61AFEF}r:x=x_0$ è un asintoto verticale per la funzione $f(x)$ se si verifica *una* di queste due condizioni:
$$\color{#61AFEF}\lim\limits_{x \to x_0^-}f(x)=\pm\infty,\quad \lim\limits_{x \to x_0^+}f(x)=\pm\infty$$
> [!example]+ <font color="orange">Esempio</font>
>$$y=\frac{x^2+2}{x-3}$$
>Questa presenta come unico asintoto verticale la retta $x=3$, come si può vedere dal grafico.
>
>![[Pasted image 20220504101416.png|500]]

## Asintoto Orizzontale
>Un **Asintoto Orizzontale** è una *retta orizzontale* che approssima il comportamento del grafico di una funzione all'infinito, ossia ad un punto $y_0$ *non definito* nella funzione.

Considerare una funzione $f:Dom(f)\to\mathbb{R}$ e supporre che il suo dominio sia [[006 Insieme Illimitato e Limitato#Insiemi Illimitati|illimitato]].

Si dice che la retta orizzontale $\color{#61AFEF}r:y=k, k\in\mathbb{R}$ è un *asintoto orizzontale bilaterale* per la funzione $f(x)$ se si verificano queste *due* condizioni:
$$\color{#61AFEF}\lim\limits_{x \to -\infty}f(x)=c,\quad \lim\limits_{x \to +\infty}f(x)=c$$

Si chiama asintoto orizzontale sinistro della funzione $f(x)$ se vale solo la prima condizione;
Si chiama asintoto orizzontale destro della funzione $f(x)$ se vale solo la seconda condizione.

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20220504103121.png|500]]

## Asintoto Obliquo
>Un **Asintoto Obliquo** è una *retta obliqua* che rappresenta l'andamento del grafico di una funzione all'infinito, ossia ad una retta che *non rientra nella funzione*.

Si considera una retta obliqua $$y=\textcolor{#FF6611}{m}x+\textcolor{#00C575}{q}$$
Dove $\textcolor{#FF6611}{m}$ si trova in questo modo:
$$\textcolor{#FF6611}{m}=\lim\limits_{x \to \infty}\frac{f(x)}{x}$$

Ci sono tre possibilità:
- Se $\color{#FF6611}m=a\in\mathbb{R}$, allora *potrebbe* esistere un asintoto obliquo, quindi si cerca $\textcolor{#00C575}{q}$
- Se $m=0$, allora non esiste
- Se $m=\infty$, allora non esiste

La $\textcolor{#00C575}{q}$ si trova in questo modo:
$$\textcolor{#00C575}{q}=\lim\limits_{x \to \infty}\left[f(x)-\textcolor{#00C575}{m}\cdot x\right]$$
Ci sono tre possibilità:
- Se $\color{#00C575}q=b\in\mathbb{R}$, allora *esiste un asintoto obliquo*
- Se $\color{#00C575}q=0$, allora *esiste un asintoto obliquo*
- Se $q=\infty$, allora non esiste

Quindi, se $\textcolor{#FF6611}{m}$ e $\textcolor{#00C575}{q}$ sono dei numeri reali, li si vanno a sostituire nella formula della retta originale [^1] e si ottiene la *retta dell'asintoto*.

[^1]: $y=\textcolor{#FF6611}{m}x+\textcolor{#00C575}{q}$

> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20220504104407.png|500]]

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/limiti-continuita-e-asintoti/3439-asintoto.html)