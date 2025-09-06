---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Funzioni
cssclasses:
  - 
---
>Le **Regole del calcolo dei Limiti** sono un insieme di formule che costituiscono la cosiddetta *Algebra dei Limiti*.

Questa consiste in un insieme di semplici regole che mettono in relazione il passaggio al [[037 Limiti di Funzioni|limite]] con le operazioni tra funzioni.
Tali formule permettono di ridurre il calcolo di limiti di funzioni in cui compaiono somme, differenze, moltiplicazioni e rapporti al calcolo di limiti più semplici.

Per rendere il calcolo ancora più semplice (o per risolverli definitivamente), quando è possibile, si utilizzano i [[040 Limiti Notevoli|limiti notevoli]].

## Regole dei Calcoli

### Limite di una Costante

$$\lim\limits_{x \to x_0}c=c, \forall c\in\mathbb{R}$$

### Somma o Differenza
Il limite della somma (o differenza) è uguale alla somma dei limiti: $$\lim\limits_{x \to x_0}(f(x)\ \pm\ g(x))=\lim\limits_{x \to x_0}f(x)\ \pm\ \lim\limits_{x \to x_0}g(x)$$
### Prodotto di una Funzione per una Costante
Il limite del prodotto di una funzione per una costante è uguale alla costante per il limite della funzione:
Ovvero, fissata $c\in\mathbb{R}$ come una costante moltiplicativa, vale la formula $$\lim\limits_{x \to x_0}(c\cdot f(x))=c\cdot \lim\limits_{x \to x_0}f(x)$$
Questa proprietà vale anche per i [[MB - 15 Logaritmi|logaritmi e logaritmi naturali]]: $$\lim\limits_{x \to x_0}(\log_b(f(x))=\log_b(\lim\limits_{x \to x_0}f(x))$$

### Prodotto
Il limite del prodotto è uguale al prodotto dei limiti: $$\lim\limits_{x \to x_0}(f(x)\cdot g(x))=\left(\lim\limits_{x \to x_0}f(x)\right)\cdot\left(\lim\limits_{x \to x_0}g(x)\right)$$
### Rapporto
Il limite del rapporto è uguale al rapporto dei limiti. In questo caso bisogna considerare un'ipotesi aggiuntiva, e supporre che $x_0$ sia un punto in cui la funzione $g(x)\neq0$:
$$\lim\limits_{x \to x_0}\frac{f(x)}{g(x)}=\frac{\lim\limits_{x \to x_0}f(x)}{\lim\limits_{x \to x_0}g(x)}$$

### Funzioni Composte
Per le [[023 Applicazioni Composte|funzioni composte]] bisogna modificare le ipotesi di base. Supponiamo che $f:Dom(f)\subseteq\mathbb{R}\to\mathbb{R}$ sia una [[035 Funzione Continua|funzione continua]] in un [[009 Intorno di un Punto#PUNTO DI ACCUMULAZIONE|punto di accumulazione]] $x_0\in Dom(f)$, che sia $y_0 = f(x_0)$ e supponiamo che $g:Dom(g)\subseteq\mathbb{R}\to\mathbb{R}$ sia una funzione continua in $y_0\in Dom(g)$.
$$\lim\limits_{x \to x_0}g(f(x))=g(\lim\limits_{x \to x_0}f(x))$$
Ovvero possiamo passare al limite direttamente nell'argomento della seconda funzione in ordine di composizione.

### Calcolo con il Teorema di De l'Hôpital
È possibile applicare Il [[055 Teorema di de l_Hôpital|teorema di de l'Hôpital]] solamente quando si ha una di queste due [[038 Forme Indeterminate dei Limiti|forme indeterminate]]: $$\left[\frac{0}{0}\right],\quad \left[\frac{\infty}{\infty}\right]$$
### Limite Notevole Reciproco
Il [[040 Limiti Notevoli|limite notevole]] reciproco ha come risultato il reciproco del risultato del limite notevole originario.

> [!example]- <font color="orange">Esempi</font>
>$$\lim\limits_{x \to 0}\frac{e^x-1}{x}=\frac{1}{1}=1,\quad\quad\lim\limits_{x \to 0}\frac{x}{e^x-1}=\frac{1}{1}=1$$
>$$\lim\limits_{x \to 0}\frac{1-cos(x)}{x^2}=\frac{1}{2},\quad\quad\lim\limits_{x \to 0}\frac{x^2}{1-cos(x)}=\frac{2}{1}=2$$
>
>Si dimostra elevando il limite notevole originale alla $-1$: $$\lim\limits_{x \to 0}\frac{x^2}{1-cos(x)}=\left(\lim\limits_{x \to 0}\frac{1-cos(x)}{x^2}\right)^{-1}=(1)^{-1}=1$$

### Limite di una Radice
Il limite di una sola [[MB - 14 Radicali|radice]] è uguale al limite del radicando: $$\lim\limits_{x \to x_0}\left(\sqrt{f(x)}\right)=\lim\limits_{x \to x_0}(f(x))$$

## Calcolo con Sostituzione Diretta
L'insieme di regole dell'algebra dei limiti e la proprietà di continuità delle funzioni individuano la cosiddetta tecnica di *calcolo dei limiti per sostituzione diretta*. $$\lim\limits_{x \to x_0}f(x)=f(x_0)$$
Dove si va semplicemente a sostituire nella funzione $f$ la $x$ ad $x_0$.

> [!example]- <font color="orange">Esempio</font>
>Calcolare: $$\lim\limits_{x \to \pi}(\sin(x)+\cos(x))$$
>$$\lim\limits_{x \to \pi}(\sin(x)+\cos(x))=\lim\limits_{x \to \pi}\sin(x)+\lim\limits_{x \to \pi}\cos(x)= \sin(\pi)+\cos(\pi)=0+(-1)=-1$$

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/limiti-continuita-e-asintoti/100-algebra-dei-limiti-quali-calcoli-si-possono-fare.html)