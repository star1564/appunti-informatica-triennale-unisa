---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Testing
cssclasses:
  - 
---
>Lo **Usability Testing** individua le *differenze* tra quello che fa il sistema e quello che gli utenti si aspettano che faccia. Quindi è un approccio dal punto di vista dell'utente.

In esso, i **partecipanti rappresentativi della popolazione degli utenti** riscontrano problemi *manipolando l'interfaccia utente o una sua simulazione*. È quindi usato anche per testare i dettagli dell'UI destinato all'utente.

- Gli sviluppatori formulano innanzitutto *una serie di obiettivi del test*, descrivendo ciò che sperano di apprendere nel test.
- Gli obiettivi del test vengono quindi valutati in *una serie di esperimenti* in cui i partecipanti vengono addestrati a svolgere compiti predefiniti.
- Gli sviluppatori *osservano i partecipanti e raccolgono dati* che misurano le prestazioni e le preferenze dell'utente per identificare problemi specifici con il sistema o raccogliere idee per migliorarlo.

Esistono tre tipi di usability test.

## Scenario Test
Viene presentato *uno [[011 Scenari e Casi d_Uso#Scenario|Scenario]]* ad uno o più utenti e viene valutato *in quanto tempo gli utenti lo comprendono*.
Lo scenario selezionato dovrebbe essere il più realistico e dettagliato possibile. È anche possibile usare dei [[004 Modelli di CVS#^920367|Mock-Up]].

| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Economici da realizzare e ripetere | Gli utenti non possono interagire direttamente con il sistema |
|  | I dati sono fissi |

## Prototype Test
Viene presentata *una parte del software* agli utenti finali che *implementa gli aspetti chiave del sistema*.

Può essere di tipo:
- **Prototipo Verticale**: implementa completamente un caso d'uso nel sistema. Vengono usati per valutare i requisiti fondamentali.
- **Prototipo Orizzontale**: implementa un singolo strato nel sistema, che presenta spesso una interfaccia per la maggior parte dei casi d'uso.

| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Fornisce una vista realistica del sistema | Richiede un impegno maggiore nella costruzione |
| Il prototipo può essere usato per collezionare informazioni dettagliate |  |

## Product Test
Questo test è simile al prototype test, ma *viene usata una versione funzionale del sistema* al posto del prototipo.

| <font color="green">Vantaggi<font> | <font color="red">Svantaggi<font> |
| ---- | ---- |
| Fornisce una vista REALE del sistema | Bisogna avere un sistema facilmente modificabile per poter <br>prendere in considerazione i risultati |


___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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