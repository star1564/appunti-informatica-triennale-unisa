---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JavaScript
cssclasses:
  - 
---
>**JavaScript** (*JS*) è un linguaggio di programmazione ad alto livello, [[001 Introduzione a OOP#AD INTERPRETAZIONE|interpretato]] e [[001 Introduzione a OOP|orientato agli oggetti]]. È ampiamente utilizzato per lo sviluppo di applicazioni web *interattive e dinamiche*. JavaScript viene solitamente incorporato direttamente all'interno del codice HTML delle pagine web e può essere eseguito all'interno del browser dell'utente. 

È un linguaggio potente e flessibile che consente di: 
- *[[030 JS ed elementi HTML|Manipolare il contenuto]]* degli elementi presenti in una pagina web;
- *Reagire ad eventi* generati dall'interazione fra utente e pagina web;
- *Validare* i dati inseriti dall'utente;
- *Interagire* con il browser.

> [!warning] Java e JavaScript sono due cose totalmente diverse
> Hanno solo adottato entrambi la **sintassi del C**.
> Inoltre non è necessario inserire `;` se si va a capo.

Come per [[024 CSS|CSS]] è possibile utilizzare un tag specifico `<script>` per includere il codice, ma è sempre meglio creare un file `.js` a parte per poi includerlo nell'[[006 Basi di HTML|HTML]].

Nelle [[016 JSP|JSP]] invece si deve scrivere il seguente codice:
```html
<script type="text/javascript"><%@include file="path/to/script.js"%></script>
```

> [!success] ["Compiler" per js online](https://playcode.io/empty_javascript)


---
### Variabili
Le variabili possono essere dichiarate usando delle parole chiave specifiche:
- ***`var`***: una variabile normale;
- ***`const`***: una costante;
- ***`let`***: una variabile con scope all'interno di un blocco.

![[Pasted image 20230615114309.png|500]]

Però, *non è necessario* inserire una parola chiave alla dichiarazione di una variabile (se si vuole una variabile normale).

> [!note] Le variabili **non possono essere dichiarate con un tipo**, perché lo assumono *dinamicamente* in base al dato attribuito
>```js
>var v    // Senza tipo
>v = 15.7 // Diventa un numero
>v = true // Diventa un booleano
>```

#### Tipi primitivi
In JS esistono pochi tipi primitivi:
- **Number**: non c'é distinzione tra interi e reali;
- **Boolean**;
- **String**;
- *Undefined*.

---
### Funzioni
Per dichiarare una funzione si utilizza la seguente sintassi:
```js
function nomeFunzione(){
	// ...
}
```

> [!warning] Attenzione
> Ricorda che tutto il codice che si trova al di fuori di una funzione verrà eseguito solamente una volta, quando viene caricato lo script.

#### Variabili nelle funzioni
Tutte le variabili dichiarate all'interno di una funzione con `var`, `let` e `const` hanno visibilità a *livello della funzione*.
Invece, le variabili senza parole chiave all'interno di una funzione hanno *visibilità globale*.

![[Pasted image 20230615114815.png|300]]
#### Variabili al di fuori di funzioni
Tutte le variabili dichiarate con `var`, `let` e `const` al di fuori di funzioni hanno visibilità globale (anche all'interno di funzioni).

---
### Uguaglianza
In JS, si possono usare due operatori per verificare l'uguaglianza:
- ***`‎ ==‎ `*** (*uguale a*) cerca di convertire e comparare due operandi con *tipi di dati differenti*.
	- Analogamente ***`!=`*** specifica la condizione di *non uguale a*.
- ***`‎ ===‎ `*** (*valore uguale con tipo uguale a*) compara solo due operandi con *tipi di dati uguali*.
	- Analogamente ***`!==`*** specifica la condizione di *non valore uguale con tipo uguale a*.

```js
if("1" == 1) // Qui ritorna true, perché "1" viene convertito in intero
	return true;
else
	return false;

if("1" === 1) // Qui ritorna false, perché sono due tipi differenti
	return true;
else
	return false;
```

___
[[000 Indice TSW|↖ Ritorna all'indice ↖]]

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
- [Fireship](https://www.youtube.com/watch?v=lkIFF4maKMU)