---
aliases: [Sottoinsieme, Sottoinsiemi, Inclusione]
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Teoria degli Insiemi
cssclasses:
  - 
---
> Un **Sottoinsieme** (*Inclusione*) è un [[001 Insieme|Insieme]] più piccolo di elementi che si trova all'*interno di un insieme più grande*.

> [!quote] Definizione Formale
> Siano $A$ e $B$ due insiemi. $A$ è un sottoinsieme di $B$ (ovvero $A$ è contenuto in $B$) se e solo se *ogni elemento di $A$ è anche elemento di $B$*. Ovvero:
> $$\textcolor{#CC241D}{A\subseteq B} \iff \textcolor{#61AFEF}{\forall x \in A, x \in B}$$
> Si legge: "[[018 Quantificatore Universale|Per ogni]] $x$ appartenente ad $A$, $x$ appartiene a $B$"

Quindi $A$ non è un sottoinsieme di $B$ se esiste almeno un elemento appartenente ad $A$ e che non appartiene a $B$. Ovvero è la [[021 Negazione di Quantificatori|negazione]] della definizione:
$$\textcolor{#CC241D}{A \nsubseteq B}\iff\neg(\forall x\in A, x \in B) \iff\textcolor{#61AFEF}{\exists x \in A, x \notin B}$$
> [!example]+ <font color="orange">Esempio</font>
>![[Pasted image 20211023134804.png|600]]
>Confrontando i tre insiemi A, B, C rispetto all'inclusione notiamo che:
>- $B \nsubseteq A$, perché ad esempio $t \in B$ ma $t \notin A$;
>- $C \subseteq A$ perché tutti gli elementi di C sono anche elementi di A, ovvero $\forall x \in C, x \in A$.

### [[001 Definizioni Varie#ASSIOMA O POSTULATO|Assioma]] su un Sottoinsieme
Dato $A$ insieme qualsiasi, è sempre vero che:
- L'[[001 Insieme#Insieme Vuoto|Insieme Vuoto]] è sempre un sottoinsieme di $A$;
	- $\emptyset \subseteq A$
- I [[001 Insieme#Singleton|Singleton]] di tutti gli elementi di $A$ sono sempre dei sottoinsiemi di $A$.
	- $\{x\} \subseteq A$, $\forall x\in A$

### Transitività dell'Inclusione
L'inclusione tra insiemi è una [[026 Relazioni di Equivalenza#Proprietà Transitiva|relazione transitiva]]:
$$(A \subseteq B) \land (B \subseteq C) \implies A \subseteq C$$

Infatti se $x\in A$, si ha $x\in B$ in quanto $A \subseteq B$, e da $B \subseteq C$ segue poi $x\in C$. Pertanto $\forall x \in A, x \in C$.

![[1fa0a981-1689-470d-81b0-861cbd9820ab.png|300]]

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
