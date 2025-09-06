---
aliases: 
  - Codominio di una Applicazione
  - Dominio di una Applicazione
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Applicazioni
cssclasses:
  - 
---
>Una **Applicazione** (o **[[011 Funzioni|Funzione]]**) è una [[014 Relazione|Relazione]] tra i due [[001 Insieme|Insiemi]], in cui ogni elemento del primo insieme $S$ è associato in modo *univoco* a un elemento del secondo insieme $T$. È indicata con $$f:S\to T$$ 
^1149c4







### Dominio e Codominio di una Applicazione

Questa associazione tra gli elementi è tale che per ogni elemento nell'*insieme di partenza* (**Dominio**), c'è **esattamente un elemento** nell'*insieme di destinazione* (**Codominio**). ^ecc0d9

> [!quote] Definizione Formale
>Sia $f\subseteq S\times T$, una relazione tra $S$ e $T$.
>
>Questa è una applicazione *se e solo se ogni elemento di $S$ ha un corrispondente in $T$ e ne ha uno solo*. Quindi:
>$$\forall x\in S, \exists!y\in T: (x,y)\in f$$
 ovvero $xfy$ ($\exists!$ = esiste solamente uno). 
 >
>L'applicazione si indica con:
> $$\color{#CC241D}f:S\rightarrow T$$

![[Pasted image 20211024155056.png|450]]

> [!example]- <font color="orange">Esempio</font>
**$R_1$** $=\{(x,y)\in\mathbb{N_0}\times \mathbb{Z}\space | \space x = y^2\}\quad$ NON È UNA APPLICAZIONE, perchè:
>Osserviamo che $2\in \mathbb{N_0}$ ma è privo di corrispondente, perchè con $(2,y)\in R_1 \iff 2 = y^2$. Ma *non esistono corrispondenze* a numeri in $\mathbb{Z}$ tale che il quadrato di y faccia 2, pertanto non può essere una applicazione.
>Possiamo inoltre notare che $4\in \mathbb{N_0}$ *ha due corrispondenze* in Z (2 e -2), pertanto anche per questo motivo non può essere una applicazione.
>
>![[Pasted image 20211024155146.png|300]]
>
>**$R_2$** $=\{(x,y)\in\mathbb{N_0}\times \mathbb{Z}\space | \space y = 3-x\}\quad$ APPLICAZIONE, perchè: 
>
>$\forall x \in \mathbb{N_0}, \exists!y\in\mathbb{Z}: xRy$

> [!tip] Riconoscere subito una applicazione
> Data una relazione con *elementi finiti ed elencati* si nota se è una Applicazione quando tutti gli elementi di $S$ si trovano *una sola volta nelle prime coordinate* delle coppie.

### Uguaglianza Applicazioni
Date due applicazioni $f:S\rightarrow T$ e $f:S_1\rightarrow T_1$, con $S, T, S_1, T_1$ insiemi non vuoti. 
Due applicazioni sono **uguali** se e solo *se hanno lo stesso dominio, lo stesso codominio e le stesse [[016 Immagine di un Elemento|Immagini]]*.

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