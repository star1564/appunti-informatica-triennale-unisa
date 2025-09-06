---
aliases:
  - DOM
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JavaScript
cssclasses:
  - 
---
L'interazione tra [[028 Basi per JavaScript|JavaScript]] e gli elementi HTML avviene attraverso il **Document Object Model** (*DOM*). 

>Il DOM rappresenta una *struttura ad albero degli elementi HTML presenti* in una pagina web e fornisce un'interfaccia che consente a JavaScript di accedere, modificare e manipolare questi elementi. ![[Pasted image 20230615151508.png|400]]

^fdd485

Quando il browser carica una pagina web, costruisce una rappresentazione del DOM basata sul codice HTML della pagina. Ogni elemento HTML diventa un "nodo" nel DOM, e JavaScript può interagire con questi nodi utilizzando metodi e proprietà specifiche.

> [!info]+ Dove inserire lo script nell'HTML
>È importante sapere dove inserire  `<script src="script.js"></script>` perché se deve vedere qualche elemento, se verrà inserito nell'header, non ci saranno elementi.
>Una idea è quello di metterlo alla fine del documento, quando il DOM ha caricato tutto l'albero degli elementi.

---
### Selezione degli elementi
JavaScript offre vari metodi per **selezionare gli elementi** nel DOM. Ad esempio:
- ***`document.getElementById(Id)`*** per selezionare un elemento tramite il suo ID; 
- ***`document.querySelector()`*** per selezionare il primo elemento corrispondente a un selettore CSS; 
- ***`document.querySelectorAll()`*** per selezionare tutti gli elementi corrispondenti a un selettore CSS.

> [!example]- <font color="orange">Esempio</font>
>```js
>var elemento = document.getElementById("idElemento");
>```

---
### Modificare gli elementi
Una volta selezionato un elemento, JavaScript consente di **modificarne le proprietà**, come il *testo*, gli *attributi* o lo *stile*. Si possono usare: 
- ***`element.innerHTML`*** per modificare il contenuto HTML di un elemento; 
- ***`element.style`*** per modificare il suo stile CSS.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230615124132.png]]
>
>HTML del pulsante: `<button onclick="changeParagraph()">Cambia paragrafo</button>`
>```js
>function changeParagraph(){
>    var elemento = document.getElementById("paragraph");
>    elemento.style.color = "red";
>    elemento.innerHTML = "Paragrafo cambiato.";
>}
>```
>![[Pasted image 20230615124543.png]]

---
### Aggiungere o rimuovere elementi
JavaScript consente di **creare nuovi elementi** HTML e di aggiungerli al DOM o di **rimuovere elementi esistenti**. Si possono usare: 
- ***`document.createElement(TagName)`*** per creare un nuovo elemento; 
- ***`element.appendChild()`*** per aggiungere un elemento figlio a un altro elemento; 
- ***`element.remove()`*** per rimuovere un elemento dal DOM.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20230615130336.png]]
>```js
>function showHideParagraph(){
>	var elemento = document.getElementById("paragrafo");
>	if(elemento != null) {
>		// Nascondi Paragrafo
>		elemento.remove();    
>	}
>	else {
>		// Mostra Paragrafo
>		elemento = document.createElement("p");
>		elemento.setAttribute("id", "paragrafo");
>		elemento.innerHTML = "Paragrafo";
>		document.body.appendChild(elemento);
>	}
>}
>```

---
### Gestire eventi
JavaScript consente di **ascoltare e gestire eventi** (come in un [[028 Action Listener|action listener in java]]) che si verificano sugli elementi HTML, come un clic del mouse o un evento di caricamento. 

Si utilizzano metodi come ***`element.addEventListener()`*** per registrare una funzione di gestione degli eventi e specificare quale azione eseguire quando si verifica un evento.

![[Pasted image 20230615152228.png|750]]

> [!example]- <font color="orange">Esempio</font>
>```js
>var header = document.getElementById("header");
>var paragrafo = document.getElementById("paragrafo");
>
>header.addEventListener("mouseover", function() {
>    if(paragrafo != null) {
>        paragrafo.style.color = "red";
>    }
>});
>
>header.addEventListener("mouseout", function() {
>    if(paragrafo != null) {
>        paragrafo.style.color = "";
>    }
>});
>```
>
>![[Pasted image 20230615140427.png]]
>
>![[Pasted image 20230615140439.png]]

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