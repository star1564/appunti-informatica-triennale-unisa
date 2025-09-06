---
aliases:
  - Insieme
  - Insiemi
  - Insieme Vuoto
  - Singleton
  - Ordine di un Insieme
  - Cardinalità di un Insieme
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Teoria degli Insiemi
cssclasses:
  - 
---
> Un **Insieme** è una *collezione di oggetti*, detti **elementi** dell'insieme. Al suo interno, l'*ordine e la ripetizione* degli elementi *non contano*.
> Gli elementi di uno stesso insieme possono essere di natura diversa.


Gli insiemi sono solitamente indicati con una lettera maiuscola $S$, mentre gli elementi vengono spesso denotati con lettere minuscole $x,y,z$.

Il seguente è un **diagramma di Venn**, utile per rappresentare gli insiemi.

![[Pasted image 20240229191143.png|400]]

Normalmente, si possono assegnare elementi ad un insieme *elencandoli* all'interno di **parentesi graffe $\{\ \}$**.

> Un insieme _privo di elementi_, si indica con $\color{#CC241D}\emptyset$ e si legge normalmente **insieme vuoto**.

^2a175b

> [!example]- <font color="orange">Esempio</font>
>L'insieme nella figura di sopra è creato in questo modo:
>
>$$S = \{3, a, 7, 4, b, 5\}$$
>
>Gli insiemi non vengono alterati dall'ordine e dalla ripetizione degli elementi. Quindi:
>$$\{3, 3, a, 5,  3, b, 4\} = \{3, a, 7, 4, b, 5\} = S$$

### Appartenenza ad un Insieme
>Si dice che un elemento $x$ **appartiene** ad un insieme $S$ se *$x$ è un elemento di $S$*. Si indica in questo modo:

$$\color{#CC241D}x\in S$$


Si dice che un elemento $y$ **non appartiene** ad un insieme $S$ se *$y$ non è un elemento di $S$*. Si indica in questo modo:
$$y\not\in S$$

### Singleton
> Un **Singleton** è un insieme in cui c'è *solamente un elemento*. Si indica con il solo elemento all'interno di parentesi graffe.
>$$\color{#CC241D}\{x\}$$

![[Pasted image 20240229193754.png|400]]

Si ha che:
- $\color{#61AFEF}x\neq \{x\}$
- $x\in\{x\}$
- $y\not\in\{x\}$, per qualunque $y\neq x$

### Assegnare elementi specificando una proprietà
>Per assegnare un insieme, invece di elencarne gli elementi, il che non è sempre possibile o conveniente, si può specificare una **proprietà** che è *comune a tutti gli elementi* che si vogliono aggiungere.
>$$S=\{x\ \textcolor{#CC241D}|\ x \text{ soddisfa una proprietà } p\}$$

La barra in mezzo, si legge "**tale che**". Quindi abbiamo che l'insieme $S$ è composto solamente da elementi $x$ tale che $x$ soddisfa una proprietà $p$.

> [!example]- <font color="orange">Esempio</font>
>$B = \{x\ \textcolor{#CC241D}|\ x \text{ è una lettera della parola casa}\} = \{a, c, s\}$
>
>Si legge: "$x$, **tale che** $x$ è una lettera della parola $casa$"


## Contare il numero di elementi
### Insieme Finito
>Un insieme si dice **finito** se i suoi elementi sono in *numero finito* (non infinito).

### Insieme Infinito
>Un insieme si dice **infinito** se i suoi elementi sono *infiniti*. 

### Cardinalità (Ordine) di un Insieme Finito
> La **Cardinalità** (*Ordine*) di un insieme finito è *il numero di elementi all'interno dell'insieme*. Si indica mettendo il nome dell'insieme tra due barre: 
> $$\color{#CC241D}|S|$$

> [!example]- <font color="orange">Esempio</font>
>- $A = \{a, b, c, d, e\} \implies |A| = 5$
>- $B = \{t\} \implies |B| = 1$
>- $|\emptyset| = 0$

#

___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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