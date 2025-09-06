---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Illuminazione
cssclasses:
  - 
---
La simulazione della **Luce** all'interno di una scena è il problema principale nella computer grafica. Questo per via della complessità dell'*interazione* tra l'illuminazione e gli oggetti della scena.

## Definizione di Luce
La luce che possiamo vedere è un tipo di onda chiamata "*radiazione elettromagnetica*". Queste onde hanno dimensioni diverse e i nostri occhi sono in grado di rilevare solo una **gamma** specifica. Questa gamma è compresa all'incirca tra 400 e 700 nanometri (nm) e definisce lo [[011 Colore|Spettro di colori visibile]].

![[Screenshot 2024-03-13 at 18-59-06.png]]

>La luce si comporta *sia come un'onda che come una particella*. Infatti si comporta come le onde dell'acqua e come piccoli *pacchetti di energia* (particelle). Queste particelle di luce sono chiamate **fotoni** e viaggiano incredibilmente veloci (300.000 chilometri al secondo) *in linea retta attraverso lo spazio vuoto*.

L'interazione della luce con i materiali è un problema complesso, infatti, quando la luce *colpisce* gli oggetti o *attraversa* qualcosa, questa interazione è come risolvere un complesso problema di trasporto.

Inoltre due raggi di luce che si intersecano non *interagiscono tra di loro*.

## Potenza Radiante
Diciamo di avere una scatola (di volume $V$) piena di luce.

>La **Potenza Radiante ($\Phi$)** rappresenta la *quantità totale di energia luminosa che attraversa la scatola ogni secondo*, misurata in watt. 

È come la velocità dell'acqua che scorre in un tubo.

![[Pasted image 20240313192059.png|800]]

Più fotoni passano attraverso la scatola ogni secondo, maggiore è la potenza radiante. 
È come avere più molecole d'acqua che scorrono attraverso il tubo, con conseguente aumento della portata.

Anche se le singole particelle di luce (fotoni) si muovono continuamente, la quantità complessiva di luce all'interno della scatola *rimane la stessa*, quindi abbiamo un *livello di luce costante*. 
È come un fiume che scorre, dove l'acqua continua a muoversi, ma la quantità totale nel letto del fiume rimane costante.

### Input e output
La luce può *entrare* nella scatola in due modi:
- **Emessa** - Piccoli pacchetti di energia luminosa (fotoni) vengono creati ed entrano nella scatola (come una lampadina che brilla).
- **Indifferenza** (inscattering) - La luce può entrare nella scatola anche rimbalzando sugli oggetti (come la luce del sole che si riflette sulle pareti).

![[Pasted image 20240313194029.png|650]]

La luce può anche *uscire* dalla stanza in tre modi:
- **Flusso** (streaming) - Una parte della luce attraversa la scatola senza interagire con nulla all'interno (si pensi alla luce che attraversa una finestra). ^afdfab
- **Fuoriuscita per dispersione** (outscattering) - La luce può rimbalzare sugli oggetti all'interno della scatola e uscire (come la luce che si riflette su uno specchio).
- **Assorbimento** - Una parte della luce viene "assorbita" dai materiali presenti nella scatola, come un panno scuro che assorbe la luce.

![[Pasted image 20240313194912.png|800]]


Ad ogni modo, la quantità totale di luce che entra nella stanza (creata all'interno e proveniente dall'esterno) *deve essere uguale alla quantità totale di luce che esce* dalla stanza (che passa direttamente, rimbalza o viene assorbita).

### Equazione della luce
Considerare un punto all'interno di uno spazio pieno di luce.

>**$\Phi(p,\omega)$** rappresenta la quantità di energia luminosa che fluisce *in quel punto specifico ($p$)* in una particolare *direzione ($\omega$)*.

Questa equazione considera tutti i modi in cui la luce interagisce in quel punto. Essa afferma che:
$$\text{luce in entrata (emissione + inscattering) = luce in uscita (streaming + outscattering + assorbimento)}$$



In computer grafica, l'obiettivo è capire *quanta luce raggiunge ogni punto della scena* (rappresentato da $\Phi(p,\omega)$). Si tratta di calcolare l'esatta quantità di luce che illumina ogni piccola area di un'immagine virtuale.

Conoscere l'esatta distribuzione della luce $\Phi(p,\omega)$ per ogni punto ci permette di eseguire un [[005 Radianza#Rendering|Rendering]] realistico della scena. Questo include effetti come *ombre*, *riflessi* e l'*aspetto dei diversi materiali* in base alla loro interazione con la luce.

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