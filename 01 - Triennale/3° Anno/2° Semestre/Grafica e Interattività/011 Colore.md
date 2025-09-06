---
aliases:
  - Spettro di colori visibile
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Teoria Grafica
cssclasses:
  - 
---
## Definizione
> [!tip]- Luce
> ![[004 Illuminazione#Definizione di Luce]]

>Il **colore di un oggetto** è [[010 Visione, Percezione e l'Occhio#La Percezione e l'Occhio|percepito]] come luce *[[005 Radianza#Riflessione|riflessa]]* dall'oggetto:
>- Un oggetto che *riflette* luce in modo omogeneo sull'intero spettro visibile è percepito di colore bianco
>- Un oggetto che *assorbe* luce in modo omogeneo sull'intero spettro visibile è percepito di colore nero
>- Un oggetto di un altro colore riflette luce la cui lunghezza d'onda ricade in uno *specifico intervallo*

![[Screenshot 2024-03-13 at 18-59-06.png]]

Il nostro occhio non è in grado di effettuare un'analisi spettrale, ma riporta una sensazione risultante dalla *combinazione di tutte le lunghezze* d'onda visibili. È inoltre impossibile comunicare la sensazione corrispondente ad un certo stimolo. Quello che possiamo comunicare è che due stimoli diversi producono la stessa sensazione.

### Tonalità
La **Tonalità** è il colore "puro" percepito dall'occhio umano, ovvero è legato ad un *ristretto intervallo o singola linea d'emissione* all'interno dello spettro visibile (VIOLET, BLUE, CYAN, ecc.).

## Colori primari
I tre **colori primari** che sono *percepiti da un'osservatore standard* sono:
- Rosso
- Verde
- Blu

![[Pasted image 20240314095521.png|250]]



### Modelli di Colore
>Un **modello di colore** è un modello matematico astratto che permette di *rappresentare i colori in forma numerica*, tipicamente utilizzando tre o quattro valori o componenti cromatiche.

Un modello di colore permette di associare ad un vettore numerico un elemento in uno spazio dei colori.

![[Pasted image 20240601093803.png|500]]

Questi sono i principali modelli:
- **RGB** (Red, Green, Blue) è utilizzato per realizzare dispositivi di proiezione quali monitors, TV e nell'elaborazione di immagini. Viene utilizzato anche per immagini satellitari. Si rappresenta con un vettore di tre numeri che rappresentano i tre colori primari, con un *intervallo da 0 a 255*.
	- Questo è un *Modello Additivo*, ovvero si addiziona luce (RGB) al nero

![[Pasted image 20240601094649.png]]

![[Pasted image 20240601094310.png|500]]

- **CMYK** (Cyan, Magenta, Yellow, Key) è utilizzato per realizzare dispositivi di stampa
	- Questo è un *Modello Sottrattivo*, ovvero si sottrae luce (CMYK) al bianco

![[Pasted image 20240601094624.png|300]]

![[Pasted image 20240601094332.png|500]]

- **HSB** (HSV) è utilizzato nell'elaborazione di Immagini. È la combinazione di *Hue* ([[#Tonalità|Tonalità]]), Saturazione (*Saturation*), Luminosità (*Brightness*).

- *YUV* è utilizzato nelle trasmissioni TV (NTSC e PAL) e nell'elaborazione di immagini
	- Sfrutta la maggiore sensibilità dell'occhio umano alla luminanza (immagini a livelli di grigio)

Ci sono due tipi di classi di immagini a colori:
- **True Colors** (colori veri) - è ottenuta mediante composizione (sottrattiva o additiva) di tre componenti (HSB, RGB, CMY, YIQ), dove ogni componente è quantizzata con un numero definito di bit
- **Pseudo-Colors** (colori falsi) - è ottenuta assegnando ad ogni intervallo di colori veri un colore medio ^895f3b

### Gamut
Il **Gamut** (o gamma di colore) è *l'intera gamma di colori che può essere prodotta* da un modello di colore. Indica anche l'insieme dei colori che possono essere realizzati dalla combinazione di tre colori primari di uno spazio di colore.

Esistono tre tipi:
- Il modello *LAB* (Luminosity, Green-Red Axis, Blue-Yellow Axis) copre tutti i colori nello spettro visibile
- Il *gamut RGB* è più piccolo del LAB, quindi alcuni colori (giallo puro, ciano puro) non possono essere visualizzati su un monitor
- Il *gamut CMYK* è il più piccolo

![[Pasted image 20240601095319.png]]

### Temperatura di colore
La **Temperatura di colore** è associata alla [[#Tonalità|tonalità]] della luce e si misura in kelvin. 

Essa è la temperatura alla quale un corpo nero (un oggetto ideale che assorbe tutta la radiazione luminosa incidente e *non la riflette*) dovrebbe essere *riscaldato* affinché la luce che emette abbia lo *stesso colore apparente della luce della sorgente*.


![[Pasted image 20240601100105.png|800]]

## Quantizzazione di bit
Un osservatore tipico può distinguere al più 2000 colori (e al più 40 livelli di grigio), quindi bisogna riuscire a trovare un modo per rappresentare i colori su monitor attraverso un numero adatto di bit. 

Questo numero si misura con i *bpp* (bit per pixel) e ogni dimensione permette di rappresentare diversi tipi di immagini:
- 1 bpp - **Immagini binarie** (bianco e nero)
- 8 bpp - **Immagini a [[#^895f3b|Colori Falsi]]**
- *8*-16 bpp - **Immagini in scala di grigio**
- 16-*24*-32-*48*-64 bpp - **Immagini a colori**

![[Pasted image 20240601101204.png|900]]


![[Pasted image 20240601101232.png|600]]


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