---
aliases:
  - Memoria Secondaria
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: File System
cssclasses:
  - 
---
La **Memoria di Massa** (o Memoria Secondaria) è il sistema di memorizzazione *non volatile* di un computer.

Le memorie secondarie moderne sono hard disk (disco magnetico o rigido) ed SSD.

### DISCO MAGNETICO
Un disco magnetico è composto da molti **piatti** impilati uno sopra l'altro in un asse, questi hanno una forma piana e rotonda come quella dei CD, e le due superfici sono ricoperte di materiale magnetico.

Le informazioni si memorizzano magneticamente sui piatti e vengono lette rilevando la configurazione magnetica memorizzata.

![[Pasted image 20221011175612.png|550]]

Una testina di lettura e scrittura è sospesa su ciascuna superficie d'ogni piatto. Le testine sono attaccate al **braccio del disco** che le muove in blocco.

La superficie di un piatto è divisa logicamente in **tracce** circolari a loro volta suddivise in **settori**.

Quando il disco è in funzione, un motore fa ruotare le piastre ad alta velocità (da 60 a 200 volte al secondo).

> [!nota]+ Velocità di trasferimento
> È la velocità con cui i dati vengono trasferiti dal disco al computer.

> [!nota]+ Tempo di accesso
> È il tempo per muovere la testina sul settore desiderato (tempo di ricerca + latenza di rotazione).

In generale il disco viene visto come un grosso array monodimensionale di blocchi logici, malgrado la sua struttura.

È importante capire come vengono considerati i settori nelle tracce. Ci sono due tipi di politiche a riguardo:
- Dispositivi **CVL** (Constant Linear Velocity), in cui la densità dei bit per traccia è uniforme, cioè le *tracce esterne contengono più settori*.
- Dispositivi **CAV** (Constant Angular Velocity), in cui la densità di bit *decresce andando dalle tracce interne verso quelle esterne*.

Lo spazio all'interno di un settore è composto da:
- *Informazioni* riguardante il settore;
- Una zona utilizzata per *immagazzinare dati*;
- La **ECC** (Error Correcting Code) che *verifica e corregge la zona dati* del settore in caso abbia subito errori o problemi.

![[Pasted image 20221011181514.png|600]]

> [!success]- Primo utilizzo di un nuovo disco
> La prima cosa che il SO fa con un disco è quello di formattarlo (crea i settori e le numerazioni). Dopo la creazione delle partizioni occorre formattarle logicamente.

#### GESTIONE BLOCCHI DIFETTOSI
Può succedere che man mano che si utilizza il disco, si possono identificare una serie di *blocchi difettosi* (per via di un problema fisico). Questi blocchi non si possono usare. In questi casi ci sono due tecniche:
- **Sector Sparing**: il controllore mette da parte i blocchi difettosi e *li sostituisce con dei blocchi di riserva*. Infatti, quando si formatta un disco, il SO non utilizza tutti i blocchi a disposizione, mettendoli in riserva.
- **Sector Slipping**: Tutti i settori compresi tra il settore danneggiato e quello di riserva immediatamente successivo vengono spostati avanti di un settore.


___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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