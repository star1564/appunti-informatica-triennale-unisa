---
aliases:
  - Media Query
  - Viewport
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: CSS
cssclasses:
  - 
---
Un Sito Web può essere visualizzato su innumerevoli dispositivi con dimensioni diverse, quindi è necessario specificare come lo si può mostrare a partire dalla dimensione dello schermo attuale.
![[Pasted image 20230615105133.png|300]]

---
### Media Query
Una **Media Query** è una funzionalità di CSS che permette di applicare stili diversi *in base alle caratteristiche del dispositivo o del viewport*, come ad esempio la larghezza dello schermo. 

Per usare una media query, bisogna scegliere il punto di interruzione desiderato attraverso **`@media`**. 

* Per esempio si vuole modificare l'aspetto del sito quando la larghezza dello schermo è *inferiore* a 600 pixel:
```css
@media (max-width: 600px) {
  /* Stili per schermi con larghezza inferiore a 600px */
}
```

* Oppure quando è *maggiore o uguale* ad un certo valore in pixel:
```css
@media (min-width: 600px) {
  /* Stili per schermi con larghezza maggiore o uguale a 600px */
}
```

- È possibile utilizzare *più media query* per specificare diverse condizioni e applicare stili diversi a diverse dimensioni dello schermo. Ad esempio, si usa una media query per dispositivi mobili con una larghezza massima di 480 pixel e un'altra media query per tablet con una larghezza compresa tra 481 e 768 pixel.

```css
@media (max-width: 480px) {
  /* Stili per dispositivi mobili con larghezza inferiore a 480px */
}

@media (min-width: 481px) and (max-width: 768px) {
  /* Stili per tablet con larghezza compresa tra 481px e 768px */
}
```

Esistono anche altre sintassi, come `height`, `aspect-ratio`, ecc.

---
### Impostare il Viewport
Un **Viewport** è la "finestra" di visualizzazione che *rappresenta l'area visibile di un documento* web all'interno di un browser. È l'area in cui il contenuto del sito web viene effettivamente mostrato all'utente. Il viewport può variare in dimensioni a seconda del dispositivo e delle impostazioni del browser.

Il meta tag `viewport` viene utilizzato per controllare come il browser visualizza il contenuto di un sito web sullo schermo di un dispositivo. Si usa in questo modo:
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- `width=device-width`: questo imposta la larghezza del viewport sullo stesso valore della larghezza del dispositivo in uso. In pratica, fa sì che il contenuto del sito si adatti alla larghezza dello schermo del dispositivo in modo appropriato.
- `initial-scale=1.0`: questo imposta il livello di zoom iniziale del viewport su 1.0, garantendo che il sito venga visualizzato a dimensioni originali senza alcun ridimensionamento automatico.

Questi valori possono anche essere cambiati (mettere un valore invece di `device-width` o `1.0`).

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
