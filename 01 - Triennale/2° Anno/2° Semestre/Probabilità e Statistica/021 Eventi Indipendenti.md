---
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Probabilità
cssclasses:
  - 
aliases:
---
>Due [[011 Evento|Eventi]] si dicono **Indipendenti** se la [[016 Definizione di Probabilità|probabilità]] della loro intersezione è uguale al prodotto delle singole probabilità. 
>$$\color{#CC241D}\mathbb{P}(A\cap B)=\mathbb{P}(A)\cdot \mathbb{P}(B)$$
>Quindi, sono indipendenti *se uno non fornisce alcuna informazione sull'altro*.

In caso contrario, ossia se la probabilità dell'intersezione è diversa dal prodotto delle loro probabilità, allora sono *eventi dipendenti* e si usa la [[020 Probabilità Composta|probabilità composta]].

> [!example]+ <font color="orange">Esempio</font>
> Sia $A$ l'evento in cui pioverà domani e sia $B$ l'evento in cui lancio una moneta. 
> Questi due eventi sono indipendenti.

Se $A$ e $B$ sono indipendenti, di conseguenza anche $A$ e $\overline{B}$ saranno indipendenti.


> [!example]+ <font color="orange">Esempio</font>
>>Scelgo un numero a caso tra $\{1,2,3,\dots,10\}$ e lo chiamo $N$. Supponiamo che tutti i risultati siano ugualmente probabili. Sia $A$ l'evento che $N$ è minore di 7 e sia $B$ l'evento che $N$ è un numero pari. $A$ e $B$ sono indipendenti?
> 
> - $A=\{ 1,2,3,4,5,6 \}$
> - $B=\{ 2,4,6,8,10 \}$
> - $A\cap B=\{ 2,4,6 \}$
> 
> Quindi:
> - $\mathbb{P}(A)=0.6$
> - $\mathbb{P}(B)=0.5$
> - $\mathbb{P}(A\cap B)=0.3$
> 
> Si ha che: $$\mathbb{P}(A\cap B)=0.3=0.6\cdot0.5=\mathbb{P}(A)\cdot\mathbb{P}(B)$$
> 
> Pertanto, i due eventi sono indipendenti.

> [!info]- Tre eventi
> Tre eventi sono indipendenti tra di loro se:
> - $\mathbb{P}(A\cap B)=\mathbb{P}(A)\cdot \mathbb{P}(B)$
> - $\mathbb{P}(A\cap C)=\mathbb{P}(A)\cdot \mathbb{P}(C)$
> - $\mathbb{P}(B\cap C)=\mathbb{P}(B)\cdot \mathbb{P}(C)$
> - $\mathbb{P}(A\cap B\cap C)=\mathbb{P}(A)\cdot \mathbb{P}(B)\cdot \mathbb{P}(C)$
>
> Questo è simile con $n$ eventi.

## Indipendenza Condizionata
>Se due eventi $A$ e $B$ sono indipendenti, e $\mathbb{P}(B)\neq 0$, allora $$\color{#CC241D}\mathbb{P}(A|B)=\mathbb{P}(A)$$

> [!quote]+ Dimostrazione
> Se $\mathbb{P}(B)>0$, dalla formula $\mathbb{P}(A|B)=\frac{\mathbb{P}(A\cap B)}{\mathbb{P}(B)}$ segue che $A$ è indipendente da $B$ se $$\frac{\mathbb{P}(A\cap B)}{\mathbb{P}(B)}=\mathbb{P}(A)$$ ovvero che $$\begin{align}\frac{\color{#61AFEF}\mathbb{P}(A\cap B)}{\mathbb{P}(B)}&=\text{[definizione di eventi indipendenti]}\\&=\frac{\color{#61AFEF}\mathbb{P}(A)\cdot\cancel{ \mathbb{P}(B) }}{\cancel{ \mathbb{P}(B) }}\\&=\mathbb{P}(A)\end{align}$$

Questo è vero anche per l'opposto $$\mathbb{P}(B|A)=\mathbb{P}(B)$$

---
> [!note] Differenza tra Eventi indipendenti e disgiunti
> | Concetto         | Significato                                        | Formule                                                                                                                               |
> | ---------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
> | **Disgiunti**    | $A$ e $B$ non possono accadere allo stesso tempo.  | $A\cap B=\emptyset$<br>$\mathbb{P}(A\cup B)=\mathbb{P}(A)+\mathbb{P}(B)$                                                              |
> | **Indipendenti** | $A$ non fornisce alcuna informazione riguardo $B$. | $\mathbb{P}(A\vert B)=\mathbb{P}(A)$<br>$\mathbb{P}(B\vert A)=\mathbb{P}(B)$<br>$\mathbb{P}(A\cap B)=\mathbb{P}(A)\cdot\mathbb{P}(B)$ |

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
- [Video (inglese), Statistics 110 - Eventi Indipendenti](https://youtu.be/P7NE4WF8j-Q?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=636)
- [YouMath](https://www.youmath.it/lezioni/probabilita/probabilita-discreta/1201-eventi-dipendenti-e-indipendenti.html)
- https://www.youtube.com/watch?v=LS-_ihDKr2M&list=PL0KQuRyPJoe6KjlUM6iNYgt8d0DwI-IGR&index=22