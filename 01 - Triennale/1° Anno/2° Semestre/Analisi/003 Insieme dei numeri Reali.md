---
aliases: 
tags:
  - corsi/matematica/analisi
paragrafo: Nozioni di base
cssclasses:
  - 
---
> [!info]- Insiemi Numerici Infiniti
> ![[004 Insiemi Numerici Infiniti#Definizione|Insiemi Numerici Infiniti]]

---
> L'insieme dei **Numeri Reali $\mathbb{R}$** è composto da numeri descritti in questo modo:
> - Sono rappresentati attraverso:
> 	- *Decimali* limitati o illimitati
> 	- *Periodici* o non periodici e 
> - Sono tutti e soli i numeri razionali e irrazionali.

### Assiomi dei Numeri Reali


> [!quote] <font color="orange">Assioma</font>
> Esiste il sistema dei numeri reali $\mathbb{R}$.

> [!quote] <font color="orange">Assioma (pL 12) - Operazioni in $\mathbb{R}$</font>
> Sono definite le operazioni di addizione ($+$) e moltiplicazione ($\cdot$) tra coppie di numeri reali.

> [!quote] <font color="orange">Assioma - Ordinamento $\mathbb{R}$</font>
> Considerare il seguente ordinamento $(\mathbb{R}, +, \cdot, \leq)$. 
> È definita la [[029 Relazioni d_Ordine|Relazione di minore o uguale]] ($\leq$) tra coppie di numeri reali: 
>1. Dicotomia: $\forall a,b\in \mathbb{R}$ si ha che $\color{#61AFEF}a\leq b \lor b\leq a$;
>2. Proprietà asimmetrica: $\color{#61AFEF}(a\leq b \land b\leq a)\Longrightarrow a = b$;
>3. Se $a\leq b$ allora vale anche $\color{#61AFEF}a+c\leq b+c$;
>4. Se $0\leq a\land 0\leq b$ allora valgono anche $\color{#61AFEF}0\leq a+b, 0\leq a\cdot b$.

> [!quote] <font color="orange">Assioma - Completezza</font>
> Siano $A$ e $B$, due [[002 Sottoinsiemi|Sottoinsiemi]] di $\mathbb{R}$, ovvero che $$A,B\subset \mathbb{R}$$
> 
> Inoltre si deve avere che:
> - $A,B\neq \emptyset$
> - $A\cap B = \emptyset$
> - $A\cup B = \mathbb{R}$
> 
> Allora: $$\forall a\in A, \forall b\in B: a\leq b\Rightarrow \exists c\in \mathbb{R}: \color{#61AFEF}a\leq c \leq b$$



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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/premesse-per-lanalisi-infinitesimale/3484-insieme-dei-numeri-reali.html)
