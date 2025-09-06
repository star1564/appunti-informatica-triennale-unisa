---
aliases:
  - Telecamera Semplice
  - Telecamera Generale
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Motori Grafici
cssclasses:
  - 
---
>Una **Telecamera** (o fotocamera) è il *punto di vista di un osservatore*. Funziona come l'equivalente digitale di una fotocamera reale, catturando la scena da un punto di vista specifico.

## Telecamera Semplice
Questa è una telecamera che *non si può muovere nella scena* al runtime.

Componenti chiave:
1. **Posizione** (COP, *Center Of Projection*) - è possibile definire *dove si trova* la telecamera nello spazio 3D della scena.
2. **Direzione** - è possibile impostare *la direzione in cui punta* la fotocamera. Ciò determina quale parte della scena "vede" la fotocamera e diventa l'immagine finale.
3. **Campo visivo** (FOV, *Field Of View*) - definisce *la larghezza della vista* catturata dalla fotocamera. Un FOV più ampio cattura una porzione più ampia della scena, simile a un obiettivo grandangolare in fotografia. Un FOV più stretto ingrandisce un'area specifica, come un teleobiettivo. Quindi è lo *zoom* dell'immagine virtuale.

![[Pasted image 20240317121403.png|600]]

Una volta impostate la posizione, la direzione e il FOV della telecamera, questa *lancia dei raggi virtuali nella scena* attraverso il [[007 Ray Tracing#^40888e|Ray Casting]]. 
Questi raggi *viaggiano in linea retta* finché non colpiscono un oggetto o i confini della scena. 

Visuale dalla telecamera:
![[Pasted image 20240317121454.png|500]]

Il colore e le informazioni degli oggetti intersecati dai raggi vengono utilizzati per determinare l'immagine finale.

### Caratteristiche aggiuntive
Le telecamere virtuali possono avere proprietà aggiuntive rispetto a quelle delle telecamere reali:
- **Profondità di campo** (*Depth of Field*) - consente di *mettere a fuoco* oggetti specifici sfocando lo sfondo, creando un effetto realistico di percezione della profondità.
- **Sfocatura in movimento** (*Motion Blur*) - simula la *sfocatura* che si verifica quando la telecamera o gli oggetti della scena sono *in movimento*.

## Telecamera Generale
Le telecamere semplici sono restrittive. Abbiamo bisogno di una telecamera *più flessibile* che sia in grado di:
- *Spostarsi* - Cambiare la propria posizione nella scena 3D come un cameraman virtuale.
- *Guardare in diverse direzioni* - Puntare verso qualsiasi parte della scena per catturare ciò che vogliamo.

Si può ottenere una **Telecamera Generale** con queste definizioni chiave:
1. **View Reference Point** (*VRP*) - Si riferisce a *dove si trova la telecamera* nel mondo 3D.
2. **View Plane Normal** (*VPN*) - Si riferisce alla *direzione in cui punta la telecamera*. Definisce la parte della scena che la telecamera "vede" e che diventa l'immagine finale.
3. **View Up Vector** (*VUV*) - In pratica indica alla telecamera *da che parte è "l'alto" nell'immagine finale*. Questo è fondamentale, soprattutto quando si inclina la telecamera ad angolo. Il VUV definisce una direzione "verso l'alto" coerente nella telecamera virtuale.

![[Pasted image 20240317122831.png]]

Visuale dalla telecamera:
![[Pasted image 20240317123152.png|600]]


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