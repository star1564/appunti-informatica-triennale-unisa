---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Applicazioni
cssclasses:
  - 
---
>Un'**Applicazione Composta** è una operazione che *combina due o più [[015 Applicazioni|applicazioni]]* per creare una nuova applicazione.

> [!quote] Definizione Formale
> Supponiamo di avere due applicazioni, $f:S\to T$ e $g:T \to V$.
> 
> Si definisce l'**applicazione composta** di $f$ e $g$, *l'applicazione di S in V* che ad ogni elemento $x\in S$ associa l'elemento di V che si ottiene applicando $g$ all'elemento $f(x)$ di T.
> $$\color{#CC241D}g\circ f: S \rightarrow V$$
> $$(g\circ f)(x) = g(f(x))$$

![[Pasted image 20211029151446.png|600]]

> [!example]- <font color="orange">Esempio</font>
>$f:x\in \mathbb{N}_0 \rightarrow x^2+10\in \mathbb{N}$
>$g:n\in \mathbb{N} \rightarrow -5n+8\in \mathbb{Z}$
>
>Quindi: $\quad g\circ f: \mathbb{N}_0 \rightarrow \mathbb{Z}$
>
>$(g\circ f)(x) = g(f(x)) = g(x^2+10)=-5(x^2+10)+8=$
>$=-5x^2-50+8=-5x^2-42$.


## PROPRIETÀ ASSOCIATIVA DELLE APPLICAZIONI
Siano $f: S\rightarrow T$, $g: T\rightarrow V$ e $h: V\rightarrow W$ applicazioni. Allora godono della proprietà **Associativa**:
> $$h\circ (g\circ f)=(h\circ g)\circ f$$

<font color="green">Dimostrazione (pL 70, 2.2.12)</font>

Le applicazioni *NON* godono della proprietà commutativa.
$f\circ g\neq g\circ f$

## COMPORTAMENTO DELLE APPLICAZIONI COMPOSTE
Le applicazioni composte hanno un comportamento particolare anche con le [[024 Applicazioni Identiche|applicazioni identiche]].

#### CON APPLICAZIONI INVERSE
Sia $f: S\rightarrow T$ un'applicazione ed $f^{-1}$ la stessa [[022 Applicazioni Inverse|applicazione inversa]]. Si ha che:
$$f\circ f^{-1}:T\rightarrow T, \quad id_T=f\circ f^{-1}$$
$$f^{-1}\circ f:S\rightarrow S, \quad id_S=f^{-1}\circ f$$

#### CONSERVAZIONE DI INIETTIVITÀ, SURIETTIVITÀ E BIETTIVITÀ
Il prodotto operativo di applicazioni "conserva" l'iniettività e la suriettività (e quindi la biettività), nel senso che:

Siano $f: S\rightarrow T$ e $g: T\rightarrow V$ applicazioni. Si ha:
$$f\space e\space g\space iniettive \Longrightarrow g\circ f\space iniettiva$$
$$f\space e\space g\space suriettive \Longrightarrow g\circ f\space suriettiva$$
$$f\space e\space g\space biettive \Longrightarrow g\circ f\space biettiva$$
<font color="green">Dimostrazione (pL 71, 2.2.14)</font>

Le seguenti implicazioni non si invertono. Riesce però:

Siano $f: S\rightarrow T$ e $g: T\rightarrow V$ applicazioni. Si ha:
$$g\circ f\space iniettiva \Longrightarrow f\space iniettiva$$
$$g\circ f\space suriettiva \Longrightarrow g\space suriettiva$$
$$g\circ f\space biettiva \Longrightarrow f\space iniettiva\space e\space g\space suriettiva$$
<font color="green">Dimostrazione (pL 71, 2.2.15)</font>


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
- Pagina Libro: 69