---
aliases: 
  - Successione Divergente
  - Successione Convergente
  - Successione Indeterminata
  - Successione Infinitesima
tags:
  - corsi/matematica/analisi
paragrafo: Limiti di Successioni
cssclasses:
  - 
---
Calcolare il [[017 Limiti|limite]] di una [[029 Successioni|successione]] è equivalente a chiedersi che tipo di comportamento ha la successione quando $n$ tende a diventare un certo valore. 
$$\lim\limits_{n \to +\infty}a_n$$

Ci sono quattro tipi di limiti di una successione.

## Divergente
### Divergente a $+\infty$

>Si dice che una successione **Diverge a $\color{#CC241D}+\infty$** se, fissato un qualunque $M\in \mathbb{R}$, si ha [[029 Successioni#Successioni con Proprietà Definitive|definitivamente]] che $$\color{#61AFEF}a_n>M$$
>Quindi, si scrive: $$\color{#CC241D}\lim\limits_{n \to +\infty}a_n=+\infty$$

Ovvero se si ha una [[031 Successioni Monotone#Successioni Monotone Strettamente Crescenti|successione monotona strettamente crescente]].

> [!example]+ <font color="orange">Esempio</font>
>$$\lim\limits_{n \to \infty}(n-1)=+\infty$$
>
>Perché, per qualsiasi $M$, questo viene eventualmente sorpassato da un termine della successione $n-1$.
>
>![[Pasted image 20220329122956.png|300]] 

### Divergente a $-\infty$

>Si dice che una successione **Diverge a $\color{#CC241D}-\infty$** se, fissato un qualunque $M\in \mathbb{R}$, si ha [[029 Successioni#Successioni con Proprietà Definitive|definitivamente]] che $$\color{#61AFEF}a_n<M$$
>Quindi, si scrive: $$\color{#CC241D}\lim\limits_{n \to +\infty}a_n=-\infty$$

Ovvero se si ha una [[031 Successioni Monotone#Successioni Monotone Strettamente Decrescenti|successione monotona strettamente decrescente]].

> [!example]+ <font color="orange">Esempio</font>
>$$\lim\limits_{n \to \infty}(-n-1)=-\infty$$
>
>Perché, per qualsiasi $M$, questo sorpassa sempre un termine della successione $-n−1$.
>
>![[Pasted image 20220329123050.png|300]]

## Convergente
>Si dice che una successione **Converge** se, fissato un qualunque numero reale $\epsilon>0$, esiste un numero reale $l\in\mathbb{R}$ tale che gode della seguente proprietà:
>$$\color{#61AFEF}|a_n-l|<\epsilon \iff l-\epsilon\leq a_n\leq l+\epsilon$$
>
>Quindi, si scrive: 
>$$\color{#CC241D}\lim\limits_{n \to +\infty}a_n=l$$

> [!example]+ <font color="orange">Esempio</font>
>$$\lim\limits_{n \to +\infty}\left(\frac{(-1)^n}{n+1}\right)+2=2$$
>
>Perché, all'aumentare della $n$, si raggiunge un immagine sempre più vicina al $2$, anche se non lo diventerà mai. 
>
>Si nota che $\epsilon$ è una certa distanza tra i punti e la retta costante. Si crea quindi un [[004 Intervalli#Intervallo Chiuso|intervallo chiuso]] $[l-\epsilon, l+\epsilon]$ nel quale possono rientrare degli elementi della successione a partire da un termine.
>![[Pasted image 20220329124004.png|400]]

### Infinitesima
La successione si dice **Infinitesima** se $l = 0$, ovvero se: $$\lim\limits_{n \to +\infty}a_n=0$$



## Indeterminata
>Se non si verifica nessuno dei casi precedenti, allora *il limite non esiste* e la successione si dice **Indeterminata**.
>$$\color{#CC241D}\not\exists \lim\limits_{n \to +\infty}a_n$$


> [!example]+ <font color="orange">Esempio</font>
>$$\not\exists\lim\limits_{n \to +\infty}(-1)^n$$
>
>Questo limite non esiste, perché i termini non si avvicinano mai ad un valore all'aumentare di $n$.
>
>![[Pasted image 20220329123648.png|300]]


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
- Pagina Libro: 105
- [Video - Elia Bombardelli](https://www.youtube.com/watch?v=Bhy5RBwtmzs)