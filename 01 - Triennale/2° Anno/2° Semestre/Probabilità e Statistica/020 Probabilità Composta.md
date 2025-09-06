---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Probabilità
cssclasses:
  - 
---
>La **probabilità composta** è la probabilità che due o più [[011 Evento|Eventi]] si verifichino *congiuntamente*, ossia è la probabilità che si realizzi l'intersezione tra gli eventi considerati.
>$$\color{#CC241D}\mathbb{P}(A \cap B) = \mathbb{P}(A)\cdot\mathbb{P}(B|A)=\mathbb{P}(B)\cdot\mathbb{P}(A|B)$$

È essenzialmente la formula inversa della [[019 Probabilità Condizionata|Probabilità Condizionata]].

Questa formula è particolarmente utile nelle situazioni in cui si conosce la probabilità condizionata, ma si è interessati *alla probabilità dell'intersezione*. 

Possiamo interpretare questa formula utilizzando un diagramma ad albero. In questa figura, otteniamo la probabilità in ogni punto moltiplicando le probabilità sui rami. Questo tipo di diagramma può essere molto utile per alcuni problemi.

![[Pasted image 20250319114542.png]]


> [!example]- <font color="orange">Esempio</font>
>>Un'azienda commercializza la propria produzione per il $\color{#61AFEF}60\%$ in Italia e per il $\color{#61AFEF}40\%$ all'estero. Il $\color{#CC241D}30\%$ dei prodotti vengono venduti online, e la <font color="#CC241D">quota rimanente</font> nei punti vendita. La <font color="#00C575">metà dei prodotti venduti online vengono venduti in Italia</font>. 
>>
>>Qual è la probabilità che un prodotto sia venduto in un punto vendita in Italia?
>
>Poniamo che $A = \{$un prodotto è venduto in Italia$\}, \overline{A} = \{$un prodotto è venduto all'estero ([[013 Complemento di un Insieme|complementare]])$\}, B = \{$un prodotto è venduto online$\}, \overline{B} = \{$un prodotto è venduto nei punti vendita$\}$. Abbiamo quindi le seguenti informazioni:
>- $\mathbb{P}(A)=\color{#61AFEF}0,60$;
>- $\mathbb{P}(\overline{A})=\color{#61AFEF}0,40$;
>- $\mathbb{P}(B)=\color{#CC241D}0,30$;
>- $\mathbb{P}(\overline{B})=1-\mathbb{P}(B)=1-0,30=\color{#CC241D}0,70$;
>- $\mathbb{P}(A|B)=\color{#00C575}0,50$;
>- $\mathbb{P}(\overline{A}|B)=1-\mathbb{P}(A|B)=\color{#00C575}0,50$.
>
>Vogliamo ricavare, dalla probabilità totale, $\mathbb{P}(A \cap \overline{B})$. 
>Notiamo che $\mathbb{P}(A)=\mathbb{P}(A\cap B)+\mathbb{P}(A\cap\overline{B})$, ovvero la somma di tutti i prodotti venduti in Italia, pertanto abbiamo che $\mathbb{P}(A\cap \overline{B})=\mathbb{P}(A)-\mathbb{P}(A\cap B)$. 
>Utilizzando la formula inversa sappiamo che $\mathbb{P}(A\cap \overline{B})=\mathbb{P}(A)-(\mathbb{P}(A|B)\cdot \mathbb{P}(B))=\textcolor{#61AFEF}{0,60}-(\textcolor{#00C575}{0,50}\cdot \textcolor{#CC241D}{0,30})=0,45$.
^38e189

### Legge delle probabilità composte (regola del prodotto)
$$\mathbb{P}(A_1 \cap A_2 \cap A_3 \cap \dots \cap A_n) = \mathbb{P}(A_1)\cdot \mathbb{P}(A_2|A_1)\cdot \mathbb{P}(A_3|A_2\cap A_1)\dots \mathbb{P}(A_n|A_{n-1} \cap \dots \cap A_1)$$

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
- [YouMath](https://www.youmath.it/lezioni/probabilita/probabilita-discreta/5182-probabilita-congiunta.html)