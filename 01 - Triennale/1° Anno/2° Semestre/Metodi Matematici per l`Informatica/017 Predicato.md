---
aliases:
  - Funzione Proposizionale
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Logica Predicativa
cssclasses:
  - 
---
>Il **Predicato** esprime una *proprietà* che un oggetto di un gruppo può avere o non avere; oppure le relazioni tra gli oggetti di un gruppo.

Il predicato viene spesso denotato con $\color{#CC241D}P(x)$, dove la $P$ è il predicato che indica la proprietà e la $x$ è la [[016 Variabile (MMI)|variabile]] soggetto.

> [!example]+ <font color="orange">Esempio</font>
>"$\color{#CC241D}x$ <font color="61AFEF">ha sostenuto l'esame di MMI</font>".
>La frase è divisa in due parti:
>- La prima parte è la $\color{#CC241D}x$, la variabile soggetto della frase, che in questo caso fa parte del gruppo degli studenti.
>- La seconda parte è "<font color="61AFEF">ha sostenuto l'esame di MMI</font>", il predicato, perché si riferisce alla proprietà che il soggetto può avere.

### Funzione Proposizionale
>Una **Funzione Proposizionale** è una frase che contiene una o più variabili, che diventa una  [[001 Logica Proposizionale|proposizione]] quando viene assegnata un valore ad ogni sua variabile. 
>
>Il predicato $P(x)$ che sarà il risultato della funzione proposizionale deve essere definito nel [[016 Variabile (MMI)#^98c8c0|Dominio del Discorso]] e deve avere valore `vero` (*T*, true) o `falso` (*F*, false).

In generale, un'asserzione che coinvolge $n$ variabili verrà rappresentata da un predicato $P(x_1, x_2, ..., x_n)$.
Inoltre i predicati possono avere *più argomenti* e possono rappresentare una *relazione tra argomenti*.

> [!example]- <font color="orange">Esempio</font>
>- $rosso(x)$, specifica se $x$ è rosso: T se l'oggetto è rosso; F se l'oggetto non è rosso.
>‎ %%← Empty Char%%
>- $sposati(x, y)$, specifica se $x$ e $y$ sono sposati: T se lo sono; F se non lo sono.
>‎ %%← Empty Char%%
>
>- Sia $P(x)$ il predicato "$x>3$". Quali sono i valori di $P(4)$ e $P(2)$? 
>	$P(4)$ = T, perché $4>3$; 
>	$P(2)$ = F, perché $2\not>3$.
>‎ %%← Empty Char%%
>- Sia $Q(x,y)$ il predicato "$x=y+3$". Quali sono i valori di $Q(1,2)$ e $Q(3,0)$?
>	$Q(1,2)$ = "$1=2+3$" = F
>	$Q(3,0)$ = "$3=0+3$" = V



___
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
