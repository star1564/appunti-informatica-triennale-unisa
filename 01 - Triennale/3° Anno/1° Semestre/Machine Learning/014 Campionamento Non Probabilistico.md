---
aliases: 
- Campionamento di Convenienza
- Campionamento a Valanga o a Palla di Neve
- Campionamento per Giudizio
- Campionamento per Quote
tags:
  - corsi/informatica/machine_learning
paragrafo: Training Data
cssclasses:
  - 
---
>Il **campionamento non probabilistico** comprende tutte le tecniche di selezione dati che *non si basano su criteri probabilistici*. 

Questo approccio abbraccia diverse metodologie, tra cui:
- *Campionamento di Convenienza (Convenience Sampling)*
- *Campionamento a Valanga o a Palla di Neve (Snowball Sampling)*
- *Campionamento per Giudizio (Judgment Sampling)*
- *Campionamento per Quote (Quota Sampling)*

È cruciale notare che i campioni derivati da queste tecniche spesso non riflettono una rappresentazione fedele dei dati nel mondo reale, portando a potenziali distorsioni nella valutazione dei dati (bias di campionamento). La consapevolezza di queste limitazioni è essenziale nel processo di campionamento non probabilistico.

## Campionamento di convenienza
>La tecnica di **Campionamento di Convenienza**, anche nota come "Convenience Sampling," prevede la selezione dei campioni in base alla loro *disponibilità immediata*. Invece di una selezione casuale, *si scelgono i campioni più facilmente accessibili*.

Questa metodologia è spesso impiegata in scenari caratterizzati da una *vasta popolazione*, dove testare tutti i membri sarebbe impraticabile. La scelta si orienta verso i campioni più accessibili o convenienti da acquisire, semplificando il processo di raccolta dati, sebbene a costo di una rappresentazione potenzialmente distorta della popolazione complessiva.

![[Pasted image 20231215163810.png|500]]


> [!success] Vantaggi
> - Rapida raccolta dei dati
> - Creazione di campioni poco costosi
> - Facilità di ricerca
> - Basso costo
> - Meno regole da seguire

> [!failure] Svantaggi
> - Bias di comportamento
> - Mancanza di varietà
> - Validità limitata
> - Difficoltà nell'individuare errori

> [!example]- <font color="orange">Esempio</font>
>Un ricercatore vuole condurre uno studio sulle esperienze dei sopravvissuti al cancro. Sfruttando il campionamento di convenienza , la raccolta dei dati avverrà attraverso l'uso di: 
>- Gruppi sui social network 
>- Conoscenze personali che hanno avuto tale problematica 
>- Sfruttare reti sociali di supporto per il cancro 
>
>L'utilizzo di questi strumenti è molto semplice e permette di ottenere le informazioni necessarie allo studio partendo da ciò che è più vicino al ricercatore

## Campionamento a Valanga
>Il **Campionamento a Valanga**, anche noto come "Snowball Sampling," è una tecnica che implica la *scelta dei campioni basandosi sui campioni già esistenti*. Inizialmente, si definisce una piccola popolazione di campioni conosciuti. Successivamente, si identificano altri campioni da includere nella popolazione, basandosi su quelli già noti.

Questa metodologia trova particolare utilizzo in situazioni in cui:
- L'accesso all'intera popolazione non è disponibile;
- L'identificazione dei campioni da selezionare risulta complessa.

![[Pasted image 20231215165700.png]]

> [!success] Vantaggi
> - Accesso a popolazioni nascoste o difficili da raggiungere
> - Riduzione costi e tempi
> - Identificazione di nuovi elementi che possono essere stati trascurati con altri metodi

> [!failure] Svantaggi
> - Bias di selezione
> - Campioni non rappresentativi
> - Difficoltà nel valutare gli errori di campionamento
> - Dipendenza dai campioni scelti nelle fasi iniziali

> [!example]- <font color="orange">Esempio</font>
>L'utilizzo di questi strumenti è molto semplice e permette di ottenere le informazioni necessarie allo studio partendo da ciò che è più vicino al ricercatore
>Supponiamo di voler studiare le abitudini di acquisto dei clienti per un grande centro commerciale. Tuttavia, è difficile ottenere un elenco completo di tutti i clienti.
>
>Utilizziamo questa tecnica per definire il dataset: 
>- Iniziamo selezionando casualmente $k$ persone presenti nel centro commerciale e chiediamo di partecipare al nostro sondaggio;
>- Dopo aver ricavato dati da questi, chiediamo loro di segnalarci altri clienti che conoscono e che potrebbero fornirci altri dati;
>- Ripetiamo questi passi e, proprio come una palla di neve , il dataset diventerà sempre più grande.

## Campionamento per Giudizio
>Il **Campionamento per Giudizio**, anche noto come "Judgment Sampling," è una tecnica in cui *i campioni vengono scelti direttamente dai ricercatori* o da esperti nel campo specifico. 

Questo metodo viene impiegato quando:
- I ricercatori ritengono che determinati elementi siano *particolarmente rilevanti* o *significativi* per lo studio in questione;
- Si stanno conducendo studi qualitativi in cui la qualità dei dati assume maggiore importanza rispetto alla rappresentatività statistica.

![[Pasted image 20231215170743.png|600]]

> [!success] Vantaggi
> - Flessibilità nella selezione
> - Utilizzo in contesti qualitativi
> - Accesso ad informazioni dettagliate

> [!failure] Svantaggi
> - Bias di selezione
> - Campioni non rappresentativi
> - Difficoltà nella replicabilità
> - Difficoltà nel calcolo dell'errore di campionamento

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di voler studiare il livello di soddisfazione dei clienti di un ristorante.
>
>Abbiamo a disposizione un set di 100 clienti. Il ricercatore sceglie 20 clienti in base a criteri soggettivi:
>- Clienti che hanno scritto recensioni positive o negative;
>- Clienti che hanno visitato il ristorante molto frequentemente in un determinato periodo di tempo.
>
>Dopo aver scelto questi campioni, il ricercatore ottiene informazioni da essi e trae conclusioni sul livello di soddisfazione complessivo dei clienti.

## Campionamento per Quote
>Il **Campionamento per Quote**, anche noto come "Quota Sampling," è una tecnica che si basa sull'*individuazione di quote per specifiche porzioni dei dati*. Queste quote sono definite in base a *caratteristiche specifiche dei dati*, e la selezione dei campioni è orientata a *soddisfare tali quote*.

Questa metodologia risulta particolarmente utile in situazioni in cui:
- Le risorse sono limitate e l'uso di altre tecniche risulterebbe impraticabile.
- È necessario raccogliere dati per *popolazioni rare o particolari*.
- Si desidera condurre una ricerca in domini specifici e per particolari compiti.

![[Pasted image 20231215171309.png]]

> [!success] Vantaggi
> - Rapidità nel raccogliere i dati
> - Contenimento dei costi
> - Non richiede particolari criteri di campionamento

> [!failure] Svantaggi
> - Bias di selezione
> - Difficoltà nell'ottenere una rappresentatività statica
> - Limitazioni della generalizzabilità dei risultati


> [!example]- <font color="orange">Esempio</font>
>Supponiamo di voler condurre un sondaggio sull'opinione dei consumatori riguardo ad un nuovo prodotto.
>
>La popolazione è composta da persone di diverse età:
>- Persone giovani (18 - 30 anni)
>- Adulti (31 - 50 anni)
>- Anziani (oltre i 50 anni)
>
>Si stabiliscono delle quote e si selezionano i campioni in maniera non casuale.
>
>Per esempio, vogliamo ottenere i dati da 10 giovani, 20 adulti e 30 anziani.


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