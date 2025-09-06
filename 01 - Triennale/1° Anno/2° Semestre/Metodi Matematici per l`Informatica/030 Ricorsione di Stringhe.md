---
aliases:
  - Alfabeto
  - Stringa
  - Stringa vuota
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Ricorsione - Stringhe
cssclasses:
  - 
---
## Alfabeto
> Un **Alfabeto** è un _[[001 Insieme#Insieme Finito|insieme finito]]_ di caratteri. Viene indicato con $\color{#CC241D}\Sigma$ (sigma).

> [!example]+ <font color="orange">Esempio</font> 
> $\Sigma=\{a, b, c, \dots, z\}$ è un alfabeto.

## Stringa
> Una **Stringa** (o _parola_) su un alfabeto è una _[[026 Sequenza|sequenza]] finita di simboli dell'alfabeto_.

> [!example]+ <font color="orange">Esempio</font> 
> $\Sigma=\{m,o\}$, $str =$ "$mom$".

### Stringa vuota
> La **stringa vuota** è una stringa che _non contiene simboli_. Viene indicata con $\color{#CC241D}\lambda$ (lambda).

> [!warning] Simbolo diverso per il corso di ETC
> Per [[000 Indice ETC|ETC]], la stringa vuota viene indicata con $\color{#CC241D}\epsilon$ (epsilon).

### Insieme delle Possibili Stringhe
> Dato un alfabeto $\Sigma$, l'insieme di **tutte le possibili Stringhe** su $\Sigma$ viene indicato con $\color{#CC241D}\Sigma^*$ .

> [!warning] $\Sigma$ e $\Sigma^*$ sono due insiemi diversi che non vanno confusi

> [!example]+ <font color="orange">Esempi</font> 
> $\Sigma=\{m,o\}$, $\Sigma^*=\{\lambda$, "$mom$", "$om$", "$mo$", , $...\}$ 
> $\Sigma=\{a\}$, $\Sigma^*=\{\lambda$, "$a$", "$aa$", "$aaa$", "$aaaa$", $...\}$

## Definizioni Ricorsive
Dove $w$ è una stringa su un alfabeto $\Sigma$ composto da lettere $x$, mentre $\lambda$ è una stringa vuota. Invece, $wx$ è il carattere $x$ [[031 Concatenazione di una Stringa|concatenato]] alla fine della stringa $w$.

> [!quote] Definizione Ricorsiva di una Stringa
> - **Passo Base**: La stringa vuota $\lambda$ è una stringa.
> - **Passo Ricorsivo**: Se $w$ è una stringa e $x\in\Sigma$ è un simbolo dell'alfabeto, allora $wx$ è una stringa.
>
>Se nel passo ricorsivo $w=\lambda$, porremo $\lambda x = x$.

> [!quote] Definizione Ricorsiva dell'insieme di tutte le possibili Stringhe
> L'insieme $\Sigma^*$ delle stringhe sull'alfabeto $\Sigma$ è definito ricorsivamente come segue.
>- **Passo Base**: $\lambda \in \Sigma^*$.
>- **Passo Ricorsivo**: Se $w\in\Sigma^*$ e $x\in\Sigma$, allora $wx\in\Sigma^*$.
>
>Se nel passo ricorsivo $w=\lambda$, porremo $\lambda x = x$.

Di solito, quando si ha $w\in\Sigma^*$, si può leggere come "$w$ è una stringa".

## Proprietà delle Stringhe
Certe proprietà delle stringhe hanno le loro definizioni ricorsive.
-   [[031 Concatenazione di una Stringa|Concatenazione di una Stringa]];
-   [[032 Lunghezza di una Stringa|Lunghezza di una Stringa]];
-   [[033 Potenza di una Stringa|Potenza di una Stringa]];
-   [[034 Numero di Occorrenze di un Carattere in una Stringa|Numero di Occorrenze di un Carattere in una Stringa]];
-   [[035 Stringa Palindroma|Stringa Palindroma]];
-   [[036 Inverso di una Stringa|Inverso di una Stringa (Reverse)]].


## SOTTOSTRINGA

Una **Sottostringa** di una stringa $s$ è una qualsiasi sequenza di simboli consecutivi di $s$.
Ovvero $w$ è una sottostringa di $s$ se esistono due stringhe $x$ e $y$ (eventualmente vuote) tali che $s=x\cdot w\cdot y$.

> [!example]+ <font color="orange">Esempio</font>
> "$567$" è sottostringa di "$\textcolor{#61AFEF}{567}89$";
> "$567$" è sottostringa di "$4\textcolor{#61AFEF}{567}8$";
> "$567$" è sottostringa di "$34\color{#61AFEF}567$".

---

[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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
