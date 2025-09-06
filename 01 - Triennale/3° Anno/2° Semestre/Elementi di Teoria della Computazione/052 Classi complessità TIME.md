---
aliases: 
  - Classi di Complessità
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Complessità di Tempo
cssclasses:
  - 
---

Si vuole classificare i linguaggi in base alla [[051 Complessità di Tempo|complessità di tempo]] di un algoritmo che li decide. Quindi, raggruppare in una stessa classe i linguaggi i cui corrispondenti [[024 Macchine Decisionali|Decider]] hanno complessità di tempo che sia un [[003 Notazione O|O-grande]] della stessa funzione.

---

>Sia $f : \mathbb{N} → \mathbb{R}^+$ una funzione, sia $\mathcal{M}$ l'insieme dei decisori, ovvero delle Macchine di Turing Deterministiche a nastro singolo e che si arrestano su ogni input. 
>
>La **classe di complessità di tempo deterministico** è chiamata $$\textcolor{#61AFEF}{TIME(f(n))}=\{L\ |\ \exists M\in\mathcal{M}\text{ che decide } L\text{ in tempo } O(f(n))\}$$

Una classe di complessità di tempo è una *famiglia di linguaggi*. La proprietà che determina l'appartenenza di un linguaggio $L$ alla classe è la complessità di tempo di un algoritmo per decidere $L$.

![[003 Notazione O#^413852]]

- $TIME(1)$
- $TIME(n)$
- $TIME(n^k)$
- $TIME(2^n)$
- ...

> [!example]- <font color="orange">Esempio</font>
>$$L=\{0^k1^k\ |\ k\ge 0\}$$
>La seguente macchina $M_2$ utilizza un metodo differente per decidere $L$ in modo asintoticamente più veloce rispetto a $TIME(n^2)$, ovvero $TIME(n\log n)$.
>
>>[!note] Descrizione di $M_2$
>>Sulla stringa input $w$:
>>1. Verifica che $w=0^t1^q$, altrimenti rifiuta $w$
>>2. Ripete le seguenti operazioni fino a quando restano sul nastro almeno un carattere $0$ ed un carattere $1$: 	
>>3. $\quad$Verifica che la lunghezza della stringa sia pari, altrimenti rifiuta $w$
>>4. $\quad$Scandisce nuovamente l'input, cancellando prima ogni secondo $0$, a partire dal primo $0$, poi cancellando ogni secondo $1$ a partire dal primo $1$ 
>>5. Se nessun carattere uguale a $0$ e a $1$ resta sul nastro, accetta. Altrimenti rifiuta.
>
>
>In ogni scansione eseguita nella fase 4, il numero totale di $0$ e $1$ rimanenti sono ridotti della metà e ogni eventuale resto viene scartato.
>
>Sia $n = |w|$. Per analizzare il tempo di esecuzione di $M_2$, per prima cosa osserviamo che le fasi 1 e 5 vengono eseguite una volta, impiegando un tempo di $O(n)$. Determiniamo poi il numero di volte in cui ogni fase viene eseguita. 
>
>Le fasi 3 e 4 fanno parte di un ciclo, l'iterazione è evidenziata al passo 2. Ogni fase del ciclo richiede tempo $O(n)$. 
>La fase 4 scarta almeno metà dei simboli $0$ e $1$ ogni volta che viene eseguita, quindi si verificano al massimo $O(log n)$ iterazioni del ciclo prima di averli cancellati tutti. Il tempo totale delle fasi 2, 3 e 4 è $O(n log n)$. Il tempo di esecuzione di M2 è $$O(n) + O(n \log n) = O(n \log n)$$
>
>Quindi $L\in TIME(n^2)$ e $L\in TIME(n\log n)$.



___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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