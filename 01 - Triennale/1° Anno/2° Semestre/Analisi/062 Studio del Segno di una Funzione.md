---
aliases:
  - Segno di una Funzione
tags:
  - corsi/matematica/analisi
paragrafo: Studio di Funzione
cssclasses:
  - 
---

> [!info] Nota (4/7) sullo [[058 Studio di una Funzione|Studio di una Funzione]]
>[[061 Studio dell_Intersezione degli assi di una Funzione|<- Passo precedente]] | [[063 Studio degli Asintoti e dei Limiti agli estremi del dominio|Prossimo passo ->]]

## Segno di una Funzione
Considerare una funzione $f: Dom(f)\to\mathbb{R}$ e sia $y=f(x)$ la sua espressione analitica.

>Con **Segno di una Funzione** si intende la panoramica di informazioni *relative al segno dei valori assunti dalla funzione*.

Infatti i valori $y=f(x)$ sono numeri reali, pertanto ciascuno di essi è caratterizzato da un segno: *negativo, nullo o positivo*.

Osservando l'[[016 Immagine di un Elemento|Immagine]] della funzione nel suo complesso, vale a dire l'insieme dei valori assunti dalla funzione sul proprio dominio, ci saranno alcuni insiemi del dominio su cui la funzione *assume valori negativi, positivi oppure vale zero*.

Determinare il segno della funzione significa quindi individuare i sottoinsiemi del dominio su cui la funzione assume valori negativi, nulli o positivi.

> [!example]+ <font color="orange">Esempio</font>
> Nella [[F02 Funzione Potenza con Esponente Pari|funzione potenza ad esponente pari]] si può notare che la funzione ha:
> - Valori positivi per $x<1$ e per $x>4$
> - Valori nulli per $x=1$ e per $x=4$
> - Valori negativi nell'intervallo $1<x<4$
>
>![[Pasted image 20220713160841.png|500]]


## Come studiare il Segno di una Funzione
Data una funzione reale di variabile reale $f: Dom(f)\to\mathbb{R}$ definita da $y=f(x)$ è possibile studiarne il segno *ponendone l'espressione analitica maggiore o uguale a zero*. Ovvero si deve risolvere $$f(x)\geq0$$

> [!note] Modo alternativo
> Oppure si risolvono i seguenti calcoli:
> - $f(x)=0$ (Eseguita nell'[[061 Studio dell_Intersezione degli assi di una Funzione#Intersezione con l'asse $x$|intersezione]] dell'asse $x$)
> - $f(x)>0$ (Per vedere dove è strettamente positiva la funzione)
> - $f(x)<0$ (Per vedere dove è strettamente negativa la funzione)

Così farcendo si sarà in grado di determinare l'insieme delle soluzioni, che equivarrà al sottoinsieme del dominio su cui la funzione assume valori positivi o nulli.

Invece per ricavare l'insieme dei punti in cui la funzione è negativa si considera l'[[013 Complemento di un Insieme|insieme complementare]] dell'insieme trovato precedentemente.

> [!example]+ <font color="orange">Esempio Caso Studio</font>
> $$y=\frac{ln(x+1)}{-2-ln(x+1)}$$
> Risolviamo la seguente disequazione, con il dominio già calcolato in precedenza $Dom(f)=(-1,\ e^{-2}-1)\ \cup\ (e^{-2}-1,\ +\infty)$:
> 
> $$\frac{ln(x+1)}{-2-ln(x+1)}\geq0$$
> 
> $$\begin{cases} ln(x+1)\geq0 \\ -2-ln(x+1)>0 \end{cases}\iff\begin{cases} x\geq0 \\ x<e^{-2}-1 \end{cases}$$
> 
> Tracciamo il [[MB - 11 Regola dei Segni per le Disequazioni#Grafico dei segni per una disequazione fratta o fattorizzata|grafico della disequazione]]:
> ![[Pasted image 20241005110927.png|800]]
> 
> Quindi il segno della funzione $f(x)$ è:
> - <font color="orange">positivo su</font> $\color{orange}(e^{-2}-1, 0)$
> - <font color="orange">nullo quando</font> $\color{orange}x=0$
> - <font color="orange">negativo su</font> $\color{orange}(-1, e^{-2}-1)\ \cup\ (0, +\infty)$



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
- [YouMath - Definizione Segno di una funzione](https://www.youmath.it/lezioni/analisi-matematica/le-funzioni-da-r-a-r-in-generale/21-il-segno-di-una-funzione-da-r-a-r.html)
- [YouMath - Studiare il Segno di una funzione](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico/180-step-4-studiare-il-segno-della-funzione.html)