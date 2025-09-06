---
aliases: 
  - Successione nulla
  - Successione con Proprietà Definitiva
tags:
  - corsi/matematica/analisi
paragrafo: Successioni
cssclasses:
  - 
---
>Le **Successioni di Numeri Reali** sono delle particolari [[011 Funzioni|funzioni]] che hanno come dominio l'insieme dei *numeri naturali $\mathbb{N}$* e come codominio l'insieme dei [[003 Insieme dei numeri Reali|numeri reali]] $\mathbb{R}$. 
>$$\color{#CC241D}f:\mathbb{N}\to\mathbb{R}$$
>Quindi, ad ogni numero naturale $n$ viene mappato *uno ed un solo* numero reale $a_n$.

In modo equivalente una successione è una [[026 Sequenza|sequenza]] di numeri reali con termini eventualmente ripetuti.

Si consideri una funzione $f:\mathbb{N}\to\mathbb{R}$. È possibile scriverne esplicitamente le associazioni, dando un nome ad ogni immagine:
$$
\begin{align*}
f:0\to f(0)&=a_0 \\
f:1\to f(1)&=a_1 \\
f:2\to f(2)&=a_2 \\
\vdots \\
f:n\to f(n)&=a_n \\
\vdots \\
\end{align*}
$$

Si può individuare univocamente una qualsiasi successione elencandone ordinatamente le immagini usando la notazione
$$\color{#CC241D}(a_0,a_1,a_2,...,a_n,...)\equiv\{a_n\}_{n\in\mathbb{N}}$$
Questo si chiama **Sostegno della Successione**, mentre la generica lettera $n$ prende il nome di **Indice della Successione**.

L'espressione successione numerica può riferirsi indistintamente alla successione intesa come <font color="61AFEF">funzione</font> o al <font color="green">sostegno della successione</font>.
$$\textcolor{#61AFEF}{f:\mathbb{N}\to\mathbb{R}}\iff\color{green}(a_0,a_1,a_2,...,a_n,...)$$

> [!example]- <font color="orange">Esempio</font>
>Si consideri la successione definita mediante l'espressione analitica $$\{18n\}_{n\in\mathbb{N}}$$ 
>I primi termini della successione sono:
>- $a_0=18\cdot0$
>- $a_1=18\cdot1=18$
>- $a_2=18\cdot2=36$
>- $a_3=18\cdot3=54$
>- $\dots$
>
>Quindi si ha $$(0, 18, 36, 54, \dots, 18n, \dots)$$
>
>---
>
>Si consideri la successione definita mediante l'espressione analitica $$\left\{\frac{1}{n^2+1}\right\}_{n\in\mathbb{N}}$$ 
>I primi termini della successione sono:
>- $a_0=\frac{1}{0^2+1}\cdot0=0$
>- $a_1=\frac{1}{1^2+1}=\frac{1}{2}$
>- $a_2=\frac{1}{2^2+1}=\frac{1}{5}$
>- $a_3=\frac{1}{3^2+1}=\frac{1}{10}$
>- $\dots$
>
>Quindi si ha $$\left(0,\frac{1}{2}, \frac{1}{5}, \frac{1}{10}, \dots, \frac{1}{n^2+1}, \dots\right)$$


### Differenze tra Funzioni e Successioni nel Grafico

L'unica differenza tra una normale funzione ed una successione è notabile nel [[012 Grafici di Funzioni|grafico]]:
- Una funzione è rappresentata da una *linea continua*, sulla quale si possono trovare i valori reali.
  
  ![[Pasted image 20240930101811.png|650]]


- Una successione è **rappresentata da punti**, questo perché si prendono in considerazione solo i numeri naturali.
  
  ![[Pasted image 20220713111245.png|650]]

### Successione Nulla

Si ha una *successione nulla* se al posto di $a_n$ si ha $0$. Infatti si ha l'espressione $f(n)=0,\ \forall n\in\mathbb{N}$. La sua immagine è il [[001 Insieme#Singleton|Singleton]] $\{0\}$. 
$$\color{#61AFEF}(0, 0, 0, ..., 0, ...)$$

---
### Successioni con Proprietà Definitive
>Si dice che una successione $\{a_n\}_{n\in\mathbb{N}}$ possiede (o acquista) una certa proprietà **Definitivamente** se esiste $m\in\mathbb{N}$ tale che $a_n$ *soddisfa quella proprietà per ogni $n\geq m$*.

> [!example]- <font color="orange">Esempio</font>
>Con la successione $\{a_n\}_{n\in\mathbb{N}}=\{\frac{1}{n+1}\}$ si ha che $$(1, \frac{1}{2}, \frac{1}{3}, \frac{1}{4}, \dots, \frac{1}{n+1}, \dots)$$
>
>Quindi $\{\frac{1}{n+1}\}$ è definitivamente minore di $\frac{1}{3}$ a partire da $n=3$.


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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/successioni/395-cosa-sono-le-successioni.html)
- [Video - Elia Bombardelli](https://youtu.be/PtQSWL-5GCo?si=eOCVAZO9NHtxsxvM)