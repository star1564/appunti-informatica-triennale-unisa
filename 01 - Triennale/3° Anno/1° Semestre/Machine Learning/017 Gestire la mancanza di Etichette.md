---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Training Data
cssclasses:
  - 
---
Esistono molte tecniche che trattano il problema della **Mancanza delle Etichette** dai dati di training.

![[Pasted image 20231215190412.png|800]]

## Weak Supervision
>Nell'approccio di **Weak Supervision**, si sfruttano delle *Euristiche* definite da persone esperte del dominio. 

Queste sono delle *regole pratiche o principi guida* che facilitano la risoluzione di problemi complessi o la presa di decisioni in situazioni specifiche. Le euristiche sono spesso utilizzate quando *non è possibile o pratico esaminare tutte le possibili soluzioni* o considerare tutti i dettagli di una decisione.

Esistono diverse tipologie di euristiche:
- **Euristiche Keyword**: si basa sull'individuazione di *parole chiavi* all'intero del campione;
- **Espressioni regolari**: queste sono basate sulla *ricerca di un match di una certa espressione regolare* definita;
- **Database lookup**: si definisce una lista di concetti. Il match avviene verificando se il campione *contiene uno o più elementi della lista*;
- **Output di altri modelli**: le predizioni di altri modelli *pre-addestrati* diventano etichette.

Le euristiche possono introdurre imprecisioni, portando a *etichette rumorose*. Pertanto, è cruciale *combinare e ricalibrare le funzioni di etichettatura* per ottenere un insieme di etichette corrette.

La weak supervision è quindi un approccio *semplice e potente*, che ci offre un buon punto di partenza per l'etichettatura dei dati, ma bisogna stare attenti al *rumore* generato dalle euristiche che definiamo.

## Semi-Supervision
>Nell'approccio di **Semi-Supervision**, si formulano *ipotesi strutturali per creare nuove etichette*, richiedendo un insieme iniziale di etichette.

In genere, queste tecniche performano in maniera ottimale quando si hanno *poche etichette per l'addestramento* dei modelli.

Però, se utilizziamo *pochi dati* durante la valutazione del modello, questo potrebbe andare in [[Overfitting]]. invece se ne utilizziamo *troppi*, l'aumento delle prestazioni ottenute scegliendo il modello migliore *potrebbe essere inferiore* all'aumento che si otterrebbe utilizzando questi dati per la fase di training.

Esistono diverse tecniche di questo tipo.

### Self-Training
>La **Self-Training** è la tecnica più comune di Semi-Supervision. Consiste nell'*addestramento di un modello sul dataset nel quale ci sono già dei dati etichettati*, per poi usarlo per fare *predizioni dei dati non etichettati*. Assumendo che le predizioni con alta probabilità siano corrette, possiamo aggiungere queste etichette al set già disponibile.

In questo modo, il dataset diviene via via *più grande* e si può sfruttare per riaddestrare il modello. Questa operazione viene ripetuta fino a quando non si ottengono modelli con performance accettabili.

![[Pasted image 20231215192505.png|800]]

### Similarity-Based
>La tecnica **Similarity-Based** consiste nel lavorare sulla *similarità* dei campioni. Si assume che i *dati con caratteristiche simili* debbano avere la *stessa etichetta*.

Questa similarità può essere valutata con metodi semplici o complessi.

![[Pasted image 20231215192750.png|800]]

### Perturbation-Based
>La tecnica **Perturbation-Based** comporta l'_intenzionale aggiunta di rumore ai dati_. Attraverso piccole modifiche (_perturbazioni_), l'aspettativa è che le etichette rimangano invariate.

Le perturbazioni possono essere di tipo diverso:
- Rumore alle immagini;
- Piccoli valori generati casualmente;
- Modificare di poco gli embedding di una serie di parole.

È importante che i dati perturbati *abbiano le stesse etichette* di quelli non perturbati.

## Transfer Learning

>Il **Transfer Learning** si riferisce all'uso di un *modello precedentemente addestrato* per un compito specifico come *punto di partenza per un nuovo compito*. 

Inizialmente, il modello di base viene addestrato su un *compito di base* che generalmente dispone di dati di addestramento *abbondanti ed economici*. Questo modello addestrato può successivamente essere impiegato per il compito di nostro interesse. 

![[Pasted image 20231215194657.png|600]]

In alcuni scenari, possiamo utilizzare *direttamente il modello di base* per eseguire il nuovo compito ([[Zero-Shot Learning]]), mentre in altri casi il modello deve essere *migliorato* per adattarsi al nuovo compito ([[Fine-Tuning]]).

Il Transfer Learning è molto utile quando *non si hanno molti dati etichettati*, ma risulta essere utile anche quando si hanno dati etichettati e si vogliono *migliorare le performance* di un modello sfruttando altri modelli molto performanti.

## Active Learning
>L'**Active Learning** mira a rendere *più efficiente* l'utilizzo dei dati etichettati. In questo contesto, l'idea chiave è che i modelli di machine learning possono raggiungere prestazioni superiori con un numero limitato di dati etichettati, specialmente quando *hanno la capacità di selezionare autonomamente su quali dati esercitarsi*.

![[Pasted image 20231215195354.png|400]]

Questo approccio è spesso denominato **query learning** poiché un modello attivo (*active learner*) invia richieste (*query*) a degli annotatori, solitamente umani, per etichettare dati non ancora annotati.

L'Active Learning è molto popolare per l'utilizzo con i *data stream e dati in real-time*.

Con questa tecnica, *i dati iniziali non vengono più etichettati in modo casuale*, invece, gli annotatori etichettano solo quelli che *ritengono più utili* per l'addestramento del modello. Questi dati vengono valutati attraverso varie metriche.

### Misura di Incertezza
La metrica più utile per tale scopo è la **Misura di Incertezza**, dove si etichettano i campioni su cui il modello *è più indeciso*. In questo modo, si presuppone che questi dati possano *aiutare* il modello a definire meglio i criteri di decisione (*decision boundary*).

### Query-by-Committee
Un'altra metrica è basata sul "*disaccordo tra più modelli candidati*" o "**Query-by-committee**", dove si ha un "*comitato*" composto da diversi modelli (in genere è lo stesso modello addestrato con parti di dataset e configurazioni diverse). 

Ognuno di questi modelli *decide quale campione etichettare* basandosi sull'incertezza della sua predizione. Una volta che si hanno tutte le predizioni, si calcola l'incertezza e si scelgono i *campioni più incerti*.

![[Pasted image 20231215200102.png|500]]



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