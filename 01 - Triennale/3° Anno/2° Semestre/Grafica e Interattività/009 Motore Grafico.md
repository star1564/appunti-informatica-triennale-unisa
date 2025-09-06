---
aliases:
  - 3D Engine
  - Gaming Engine
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Motori Grafici
cssclasses:
  - 
---
>Il **Motore Grafico** (3D Engine, Gaming Engine) è il nucleo software di un videogioco o di qualsiasi altra applicazione con grafica in tempo reale.
>Fornisce le *tecnologie di base*, semplifica lo sviluppo, e spesso permette al gioco di funzionare su piattaforme differenti.

Le funzionalità di base tipicamente fornite da un motore grafico includono:
- Un *Motore di [[005 Radianza#Rendering|Rendering]]* ("renderer") per grafica 2D e 3D
- Un *Motore Fisico* o rilevatore di collisioni
- Suono
- Scripting
- Animazioni
- Intelligenza artificiale per i personaggi non giocanti
- Networking
- Scene-graph

> [!note] [[U_001 Unity|Unity]]

## Tipologie
### Motori 3D in tempo reale
Questi sono utilizzati per *generare immagini tridimensionali* in modo **istantaneo**. 

Possono mostrare immagini sullo schermo (o su altri dispositivi ottici) a velocità elevata, *comunemente a 30-60 [[Frames per Second|FPS]]*. Questa velocità è ottenuta tramite varie tecniche, specialmente grazie agli *acceleratori grafici 3D*, che consistono in coprocessori matematici con memoria RAM dedicata. Questi acceleratori eseguono calcoli matematici ottimizzati, *alleggerendo la CPU* e consentendo la creazione di motori grafici più avanzati. 

Esistono due sottocategorie di motori in tempo reale: 
- **Motori grafici software** - che *usano solo la CPU* per i calcoli geometrici; 
	- Questi offrono precisione ma sono lenti.
- **Motori grafici hardware** - che sfruttano *acceleratori dedicati e GPU* (schede grafiche) per molte funzioni principali come trasformazioni, illuminazione e applicazione delle texture. 
	- Questi sono veloci ma richiedono hardware dedicato e la qualità può variare.

Questi motori sono ampiamente utilizzati nei videogiochi, simulatori, interfacce grafiche e realtà virtuale.

### Motori 3D fotorealistici
Hanno il compito di *produrre immagini ad elevata qualità* o persino inconfondibili con la realtà.

Per fare questo, impiegano algoritmi di rendering estremamente complessi ed onerosi, dove il realismo ha priorità sulle performance di calcolo.

Vengono usati soprattutto per effetti speciali e film d'animazione.

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