---
aliases:
  - Eventi Compatibili
  - Eventi Incompatibili
  - Eventi Disgiunti
  - Eventi Complementari
  - Eventi Due a Due Incompatibili
  - Eventi Due a Due Disgiunti
tags:
  - corsi/matematica/probabilità_statistica
inglese:
  - Eventi
paragrafo: 
cssclasses:
  - 
---
Consideriamo uno [[010 Spazio Campionario|Spazio Campionario]] $S$ e due [[011 Evento|Eventi]] $E_1, E_2$.

## Eventi Compatibili

>Diciamo che $E_1,E_2$ sono **eventi compatibili** se il verificarsi dell'uno *non impedisce il verificarsi dell'altro*, ossia se possono realizzarsi allo stesso tempo.
>
>In modo equivalente, sono compatibili se e solo se la loro intersezione non è l'[[011 Evento#Eventi Certi ed Impossibili|Evento Impossibile]].
>$$\color{#CC241D}E_1\cap E_2\neq \emptyset$$


![[Pasted image 20250311091751.png]]

## Eventi Incompatibili (o Disgiunti)
>Diciamo che $E_1,E_2$ sono **eventi incompatibili** (o disgiunti) se il verificarsi dell'uno *impedisce il verificarsi dell'altro*, ossia se non possono realizzarsi allo stesso tempo.
>
>In modo equivalente, sono compatibili se e solo se la loro intersezione è l'evento impossibile:
>$$\color{#CC241D}E_1\cap E_2= \emptyset$$

![[Pasted image 20250311093137.png]]

> [!example]- <font color="orange">Esempio</font>
>Un'urna contiene 10 palline, di cui 5 rosse e 5 bianche. Sappiamo inoltre che le palline rosse sono numerate da 1 a 5 e che le palline bianche sono numerate da 6 a 10. Si consideri l'esperimento casuale che consiste nell'estrazione di una pallina dall'urna e siano $E_1,E_2,E_3$ i seguenti eventi:
>- $\color{#ed7d31}E_1 =\{1,2,3,4,5\}$, viene estratta una pallina rossa
>- $\color{#70ad47}E_2 =\{2, 4, 6, 8, 10\}$, viene estratta una pallina con un numero pari
>- $\color{#61AFEF}E_3=\{5\}$, viene estratta la pallina numero 5
>
>![[Pasted image 20250311092937.png]]
>
>Bisogna analizzare le coppie di eventi e stabilire se si tratta di eventi compatibili o non:
>- $\textcolor{#ed7d31}{E_1} \cap \textcolor{#70ad47}{E_2} = \{2,4\}$ sono eventi compatibili, questo perché le palline rosse sono numerate da 1 a 5, dunque tra di esse vi sono due palline con un numero pari (2 e 4)
>- $\textcolor{#ed7d31}{E_1} \cap \textcolor{#61AFEF}{E_3} = \{5\}$ sono eventi compatibili, questo perché la pallina 5 è rossa
>- $\textcolor{#70ad47}{E_2} \cap \textcolor{#61AFEF}{E_3} = \emptyset$ sono eventi incompatibili, questo perché 5 è dispari

### Eventi Due a Due Incompatibili (o Disgiunti)
Consideriamo più di due eventi $E_1,E_2,\dots,E_n\subseteq S$. 

>Diciamo che gli $n>2$ eventi sono incompatibili se sono **due a due incompatibili** (o disgiunti), ovvero che se per *ciascuno di essi* il suo verificarsi impedisce il verificarsi di ciascuno degli altri.
>$$E_i\cap E_j = \emptyset,\quad \forall i \neq j$$

> [!example]- <font color="orange">Esempio</font>
>Nell'esperimento casuale del lancio di un dado, abbiamo i seguenti eventi:
>- $\color{#ed7d31}E_1=\{1\}$
>- $\color{#70ad47}E_2=\{3, 4\}$
>- $\color{#61AFEF}E_3=\{2,5,6\}$
>
>![[Pasted image 20250311093759.png]]
>
>Questi sono eventi incompatibili, in quanto sono due a due incompatibili:
>- $\textcolor{#ed7d31}{E_1} \cap \textcolor{#70ad47}{E_2} = \emptyset$
>- $\textcolor{#ed7d31}{E_1} \cap \textcolor{#61AFEF}{E_3} = \emptyset$
>- $\textcolor{#70ad47}{E_2} \cap \textcolor{#61AFEF}{E_3} = \emptyset$


## Eventi Complementari
>Diciamo che $E_1,E_2$ sono **eventi complementari** se soddisfano le seguenti condizioni:
>1. Il verificarsi dell'uno esclude il verificarsi dell'altro, ovvero che devono essere incompatibili
>2. Uno dei due si realizza di sicuro, ovvero che la loro unione è l'[[011 Evento#Eventi Certi ed Impossibili|Evento Certo]]
>
>In modo equivalente:
>$$\color{#CC241D}\begin{cases} E_1\cap E_2= \emptyset \\ E_1\cup E_2 = S \end{cases}$$

![[Pasted image 20250311093307.png]]

> [!example]- <font color="orange">Esempio</font>
>Nell'esperimento casuale del lancio di un dado a sei facce, in cui siamo interessati al numero che esce a seguito del lancio, scegliamo come spazio campionario $$S=\{1,2,3,4,5,6\}$$
>
>---
>
>I seguenti sono eventi complementari:
>- $E_1=\{1,2,3\}$, il dado mostra una faccia con un numero minore o uguale di 3
>- $E_2=\{4,5,6\}$, il dado mostra una faccia con un numero maggiore di 3
>
>Infatti: 
>- Sono eventi incompatibili, $E_1\cap E_2=\emptyset$
>- La loro unione è lo spazio campionario, $E_1\cup E_2=\{1,2,3,4,5,6\}=S$
>
>---
>
>I seguenti NON sono eventi complementari:
>- $E_3=\{1,2,3\}$, il dado mostra una faccia con un numero minore o uguale a 3
>- $E_4=\{5,6\}$, il dado mostra una faccia con un numero maggiore di 4
>  
> Infatti:
> - $E_3\cap E_4=\emptyset$
> - Però, $E_3\cup E_4=\{1,2,3,5,6\}\neq S$


___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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
- [YouMath](https://www.youmath.it/lezioni/probabilita/probabilita-discreta/1200-eventi-compatibili-e-incompatibili.html)