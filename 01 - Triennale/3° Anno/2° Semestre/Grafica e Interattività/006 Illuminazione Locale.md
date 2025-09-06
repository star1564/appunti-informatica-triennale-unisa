---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Illuminazione
cssclasses:
  - 
---
>L'**Illuminazione Locale** è un metodo utilizzato nella computer grafica per determinare come la *luce interagisce con gli oggetti* in una scena. È fondamentale per creare effetti di illuminazione realistici, come ombre, riflessi e diverse apparenze dei materiali.

È composto da:
- L'intensità della *luce finale* che raggiunge i nostri occhi dopo essersi riflessa su un oggetto (ciò che vediamo in definitiva).
- L'intensità *iniziale* della sorgente luminosa stessa.
- La quantità di luce che [[005 Radianza#Riflessione|riflette]] un particolare *materiale* invece di essere assorbito (varia in base alle proprietà del materiale).

L'unione di questi tre modelli di illuminazione crea i diversi effetti di illuminazione.

![[Pasted image 20240314095012.png]]

> [!info]+ Illuminazione globale
> Nel mondo reale, la luce rimbalza e interagisce con tutto ciò che è presente nella scena. Questa complessa interazione crea effetti come ombre morbide, illuminazione indiretta e oggetti illuminati dai riflessi di altri oggetti.
> 
> ![[Pasted image 20240314095155.png|450]]

^a27a90


## Ambiente
>L'**Ambiente** rappresenta una *luce generale di basso livello* che riempie la scena *raggiungendo tutti gli oggetti*. Fornisce un'illuminazione e una *colorazione* di base. Infatti, la luce viene spesso separata in tre [[011 Colore#Colori primari|colori primari]]: **Rosso, Verde e Blu**.

![[Pasted image 20240314095521.png|250]]

I calcoli dell'illuminazione locale considerano questi singoli colori per *determinare l'aspetto finale* dell'oggetto.

## Diffusa
>La **Diffusa** si riferisce alla *dispersione della luce in tutte le direzioni* dopo aver colpito una superficie. 
>
>![[Pasted image 20240314104300.png]]
^f6b835


>La **Luce Diffusa** invece si riferisce all'*intensità luminosa complessiva* che viene riflessa a causa della dispersione in tutte le direzioni.




In caso di molteplici fonti luminose, per determinare la luce diffusa finale, *si sommano i contributi di tutte le fonti luminose*.


###  Legge di Lambert
Questa legge afferma che la quantità di luce riflessa in una particolare direzione ($d$) è *proporzionale* al [[FT2 Funzione Coseno|coseno]] dell'angolo ($\theta$) tra la sorgente luminosa e la **normale** della superficie (una linea *perpendicolare alla superficie*, $N$).
 
![[Pasted image 20240314100148.png|800]]

## Speculare

>La **Speculare** descrive la *riflessione speculare della luce* su una superficie liscia.
>
>![[Pasted image 20240314104721.png]]

^14b133


### Perfettamente Speculare
![[Pasted image 20240314101634.png|400]]
Sebbene questo tipo di speculare sia possibile, spesso non è pratico perché può creare scenari irrealistici in cui è visibile solo l'evidenziazione speculare, trascurando l'interazione complessiva della luce con la superficie.

### Specularità Imperfetta (Phong)
Si tratta di un metodo popolare utilizzato per rappresentare la specularità imperfetta nella comuputer grafica.

![[Pasted image 20240314102122.png|400]]


Considera i seguenti fattori:    
- *Direzione della sorgente luminosa ($L$)* - Una linea che punta verso la sorgente luminosa.
- *Direzione dell'occhio ($E$)* - Rappresenta la linea che va dall'oggetto all'occhio dell'osservatore.
- *Normale della superficie ($N$)* - È una linea perpendicolare alla superficie nel punto di riflessione.
- *Vettore di mezzo ($H$)* - Viene calcolato facendo la media tra la direzione della luce ($L$) e la direzione dell'occhio ($E$).

>Il **modello di Phong** considera l'angolo *tra il vettore di mezzo ($H$) e la normale alla superficie ($N$)*. Più questo angolo è vicino a $0$ gradi, più forte è l'*evidenziazione speculare*.

### Componente speculare
Si riferisce al contributo dell'*evidenziazione speculare* al calcolo complessivo dell'illuminazione. È influenzata da un valore chiamato $m$ che rappresenta la **sottilità** del materiale.

![[Pasted image 20240314103226.png]]

Quindi:
- Alta lucentezza **($m$ alto)**: crea un'evidenziazione speculare *più piccola e più nitida* (come una superficie altamente lucida in cui la luce si riflette in un fascio più concentrato).
- Lucentezza bassa **($m$ basso)**: si ottiene un'evidenziazione speculare *più grande e più sfocata* (come una superficie meno lucida in cui il riflesso è distribuito su un'area più ampia).


___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

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