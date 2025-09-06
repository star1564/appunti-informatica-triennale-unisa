---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Deterministici
cssclasses:
  - 
---
>Un [[001 Linguaggio|linguaggio]] $A$ è detto **Regolare** se e solo se esiste un [[004 Automi Finiti Deterministici|DFA]] $M$ che lo [[005 Linguaggio di un DFA#Definizione Formale del Linguaggio Riconosciuto da un DFA|riconosce]], ovvero se $$A=\mathcal{L}(M)$$
>È importante sapere che *non tutti i linguaggi sono regolari*.

> [!tip] Tutti i [[001 Linguaggio#Linguaggi Finiti ed Infiniti|linguaggi finiti]] sono dei linguaggi regolari
> Infatti, anche $\emptyset$ è regolare.

---

> [!example]- <font color="orange">Esempio</font>
>$L=\{a^nb\ |\ n\geq0\}=\{b,ab,aab,aaab,\dots\}$ è regolare, perché esiste il seguente DFA che riconosce $L$.
>
>![[Pasted image 20240310192904.png]]
>Per essere certi che il DFA disegnato *sia corretto*, bisognerebbe fare una [[046 Principio di Induzione Matematica|prova per induzione]] usando la [[006 Funzione di Transizione Estesa DFA|Funzione di Transizione Estesa]], ovvero che $$w\in A\Leftrightarrow \hat\delta(q_0,w)\in F$$

## Chiusure su Linguaggi Regolari
La classe $\color{#CC241D}REG$, contenente tutti i linguaggi regolari, è [[Chiusura di un_opearzione|chiusa]] rispetto alle seguenti *operazioni regolari*:
- **[[008 Unione tra Insiemi|Unione]]**: $L_1\cup L_2$ - *[[008 Dimostrazioni Chiusure con DFA#Unione|Dimostrazione chiusura linguaggi unione (DFA)]]* - *[[013 Dimostrazioni Chiusure con NFA#Unione|(NFA)]]*
- **[[009 Intersezione tra Insiemi|Intersezione]]**: $L_1\cap L_2$ - *[[008 Dimostrazioni Chiusure con DFA#Intersezione|Dimostrazione chiusura linguaggi intersezione (DFA)]]*
- **[[013 Complemento di un Insieme|Complemento]]**: $\overline{L}$ - *[[008 Dimostrazioni Chiusure con DFA#Complemento|Dimostrazione chiusura linguaggi complemento (DFA)]]*

‎ %%← Empty Char%%

- **[[002 Operazioni su Linguaggi#Concatenazione (o prodotto) di Linguaggi|Concatenazione di linguaggi]]**: $L_1\circ L_2$ - *[[013 Dimostrazioni Chiusure con NFA#Concatenazione|Dimostrazione chiusura linguaggi concatenazione (NFA)]]*
- **[[003 Chiusura di Kleene|Chiusura di Kleene (Star)]]**: $L^*$ - [[013 Dimostrazioni Chiusure con NFA#Star|Dimostrazione chiusura linguaggi star (NFA)]]
- **[[002 Operazioni su Linguaggi|Reverse]]**: $L^R$
- **Suffissi**: dove una stringa deve terminare per una certa sequenza di caratteri
- **Prefissi**: dove una stringa deve cominciare per una certa sequenza di caratteri

---
> [!tip]+ Ogni linguaggio ha un sottoinsieme che è un linguaggio regolare
> Infatti, ogni linguaggio ha come sottoinsieme il vuoto, che è un linguaggio regolare.

> [!tip]+ Ogni linguaggio è un sottoinsieme proprio di un linguaggio regolare
> Infatti, sia $L$ un linguaggio sull'alfabeto $\Sigma$, sia $\Sigma'=\Sigma\cup\{a\}$, dove $a$ è un carattere che non appartiene a $\Sigma$. 
> Il linguaggio $L$ è un [[003 Sottoinsieme Stretto|Sottoinsieme Proprio]] del linguaggio regolare $\Sigma'^*$.

> [!error] In generale NON è vero che per ogni linguaggio regolare tutti i suoi sottoinsiemi propri sono regolari


> [!question]+ Sia $M$ un linguaggio regolare. Consideriamo $L$ l'insieme delle parole di $M$ che hanno lunghezza multipla di 3. $L$ è regolare?
>
>
>Sì, infatti si ha $$L=M\cap \{\Sigma^3\}$$
>Dove $\Sigma^3$ è l'insieme *finito* delle parole con [[032 Lunghezza di una Stringa|lunghezza]] 3 sull'alfabeto $\Sigma$.
>
>Quindi $L$ è regolare, perché:
>- $M$ è regolare
>- $\Sigma^3$è regolare perché è la [[003 Chiusura di Kleene|chiusura di Kleene]] di un linguaggio finito (quindi regolare)
>
>Pertanto, $L$ è l'intersezione di due linguaggi regolari.


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
- [lydia - Video](https://www.youtube.com/watch?v=nIp604p0M8M&list=PLhqug0UEsC-IDomfNsn8e3neoy34o8oye&index=5)