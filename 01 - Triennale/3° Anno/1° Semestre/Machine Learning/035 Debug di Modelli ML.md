---
aliases:
  - AutoML
tags:
  - corsi/informatica/machine_learning
paragrafo: Model Development and Training
cssclasses:
  - 
---
Il **debug** è parte integrante dello sviluppo di qualsiasi software. I modelli di ML non fanno eccezione.

### Complessità del debug per ML

- **I modelli di ML falliscono silenziosamente**: il codice viene compilato, la loss diminuisce come dovrebbe, vengono invocate le funzioni corrette e le predizioni vengono eseguite, ma *sono sbagliate*.

- **La lentezza con cui si può verificare se un bug è stato risolto**: quando si esegue il debug di un programma software tradizionale, è possibile apportare modifiche al codice e testarlo immediatamente. Tuttavia, quando si apportando modifiche a un modello di ML, *è necessario riaddestrare il modello* e attendere che converga per verificare se il bug è stato risolto, il che può richiedere ore o giorni.

- **Complessità inter-funzionale**: in un sistema di ML ci sono molti componenti (dati, algoritmi, ecc.), pertanto quando si verifica un errore *potrebbe essere difficile stabilire chi lo ha causato*.

### Cause del fallimento di un modello ML

- **Vincoli teorici**: ogni modello ha le proprie ipotesi sui dati e sulle feature che utilizza. Un modello potrebbe fallire perché i dati da cui apprende *non sono conformi alle sue ipotesi*. 
	- Ad esempio, si utilizza un modello lineare per dati i cui confini decisionali non sono lineari.

- **Scelta inadeguata degli [[Iper-parametro|iper-parametri]]**: dato un modello, un insieme di iper-parametri può fornire un risultato all'avanguardia, ma un altro insieme potrebbe far sì che il modello non converga mai. Il modello si adatta perfettamente ai dati e la sua implementazione è corretta, ma *una serie inadeguata di iper-parametri potrebbe rendere il modello inutile*.

- **Problemi di dati**: campioni di dati ed etichette non correttamente accoppiati, etichette rumorose, feature normalizzate usando statistiche obsolete, ecc. potrebbero *influenzare negativamente il training* di un modello.

- **Scarsa scelta di feature**: i modelli possono avere molte feature da cui apprendere. Un numero eccessivo di feature potrebbe far sì che un modello si adatti eccessivamente ai dati, mentre un numero insufficiente *potrebbe mancare di potere predittivo* per consentire a un modello di fare buone previsioni.

## AutoML
L'**AutoML** si riferisce all'*automatizzazione del processo di ricerca di algoritmi di ML* per risolvere problemi del mondo reale.

### Soft AutoML
Una forma leggera, e la più popolare, di AutoML in produzione è la regolazione degli iper-parametro.

Con diversi set di iper-parametri, *lo stesso modello può fornire performance drasticamente diverse sullo stesso set di dati*. Infatti, i modelli più "deboli" con iper-parametri ben tarati possono superare i modelli più "forti".

L'obiettivo della regolazione degli iper-parametri è *trovare l'insieme ottimale di iper-parametri per un dato modello* all'interno di uno spazio di ricerca.

Quando si regolano gli iper-parametri, bisogna tenere presente che *le performance di un modello potrebbero essere più sensibili* alla variazione di un iper-parametro rispetto a un altro, e quindi gli iper-parametri sensibili dovrebbero essere regolati con maggiore attenzione.

> [!tip]+ Framework popolari
> I framework di ML più noti sono dotati di funzionalità integrate o di terze parti per la regolazione degli iper-parametri, ad esempio Scikit Learn con Auto Sklearn, TensorFlow con Keras Tuner e Ray con Tune.


___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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