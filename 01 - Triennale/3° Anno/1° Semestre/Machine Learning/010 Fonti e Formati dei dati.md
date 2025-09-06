---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Data Engineering
cssclasses:
  - 
---
L'ascesa del machine learning negli ultimi anni è strettamente legata all'aumento dei [[Big Data]], ottenuti da varie Fonti (Sorgenti) di Dati.

## Fonti di dati
>Una **Fonte Dati** si compone di informazioni provenienti da diverse fonti, suddivise principalmente in tre categorie:

1. **Dati immessi dagli utenti (testo, immagini, video, file)**:
	- Gli utenti possono inserire *dati errati*, quali testi eccessivamente lunghi o brevi, valori numerici al posto del testo e file con formati non corretti;
	- L'input utente richiede processi aggiuntivi di *verifica* e *validazione* per garantire la *qualità dei dati*;
	- È cruciale garantire una rapida elaborazione di tali dati, considerando la limitata pazienza degli utenti nell'attesa dei risultati.

2. **Dati generati dai sistemi (log di sistema)**:
	- I log registrano lo stato e gli eventi rilevanti del sistema, come l'utilizzo della memoria, il numero di istanze, i servizi chiamati, i pacchetti utilizzati, ecc;
	- Questo tipo di dati è meno incline a essere malformato rispetto ai dati immessi dagli utenti;
	- Anche se sono dati generati dal sistema, vengono trattati come parte dei dati dell'utente e potrebbero essere soggetti alle normative sulla privacy.

3. **Database aziendali generati dalle applicazioni aziendali**:
	- Questi database gestiscono vari asset aziendali, tra cui inventario, relazioni con i clienti, utenti, ecc;
	- Questi dati possono essere utilizzati direttamente da modelli di machine learning o da vari componenti di un sistema di machine learning.

### Tipologie di Dati
Ci sono tre tipi di modi in cui si possono ottenere dati.
- **Dati di prime parti:** si riferiscono ai dati *direttamente raccolti dalle aziende dai propri clienti*.
	- Questi dati offrono un'immagine diretta del comportamento e delle preferenze dei clienti nei confronti dei prodotti o servizi offerti.

- **Dati di seconde parti:** si riferiscono ai dati raccolti dalle aziende *sui propri clienti*, *resi successivamente disponibili per collaborazioni* o partnership con altre entità.
	- Questi dati consentono alle aziende di arricchire le loro conoscenze sui clienti attraverso l'accesso a informazioni provenienti da fonti esterne.

- **Dati di terze parti:** rappresentano dati sui clienti raccolti da *terze parti specializzate nella raccolta di informazioni* su individui che *non sono direttamente clienti dell'azienda*.
	- L'utilizzo di dati di terze parti è stato notevolmente influenzato dalle nuove regolamentazioni sulla privacy dei dati.

Questi dati sono particolarmente preziosi per sistemi avanzati come i sistemi di raccomandazione, che sfruttano informazioni dettagliate sugli interessi degli utenti per generare risultati personalizzati. 

## Formati dei dati
Data l'eterogeneità delle fonti dati e dei relativi modelli di accesso, l'archiviazione dei dati diventa una sfida complessa. È cruciale progettare il formato di archiviazione con una prospettiva futura, tenendo conto dell'*utilizzo previsto dei dati*, inclusi quelli multimediali, garantendo coerenza tra diversi hardware.

Nel considerare un formato di dati, è possibile valutare diverse caratteristiche, tra cui:
- **Leggibilità**: assicurarsi se i dati debbano essere *comprensibili* e interpretabili senza difficoltà da un umano;
- **Modelli di accesso**: garantire un *accesso efficiente* e flessibile ai dati, tenendo conto dei vari requisiti di utilizzo;
- **Tipo di rappresentazione**: scegliere tra rappresentazione *binaria*, *testuale*, o altre, influenzando la dimensione dei file e l'efficienza di trasmissione e archiviazione.

### Formati di File per i Dati

| Formato                          | Binario/Testo      | Leggibile | Dove può essere utilizzato    |
|:-------------------------------- |:------------------ |:--------- |:----------------------------- |
| [[036 JSON con AJAX#JSON\|JSON]] | Testo              | Si        | Ovunque                       |
| **CSV**                              | Testo              | Si        | Ovunque                       |
| **Parquet**                          | Binario            | No        | Hadoop, Amazon Redshift       |
| Avro                             | Binario (primario) | No        | Hadoop                        |
| Protobuf                         | Binario (primario) | No        | Google, TensorFlow            | 
| Pickle                           | Binario            | No        | Python, PyTorch serialization |

> [!failure] I problemi del JSON
>I principali ostacoli associati al formato JSON sono i seguenti:
>1. Modificare uno schema dopo che i dati sono stati formattati in file JSON può risultare complesso e laborioso;
>2. In quanto file di testo, i documenti JSON occupano uno spazio considerevole.


I formati CSV e Parquet delineano due approcci nettamente differenti:
- **CSV** (*Comma Separated Values*): segue un approccio di tipo "*row major*", dove gli elementi consecutivi di una riga sono memorizzati in *sequenza*. ^04aac2

![[Pasted image 20231215161530.png]]

- **Parquet**: adotta un paradigma di tipo "*column major*", organizzando gli elementi consecutivi di una colonna in una disposizione *contigua*.

![[Pasted image 20231123090756.png|800]]

In generale, i formati **row major** sono ideali per *frequenti operazioni di scrittura*, poiché sfruttano l'ottimizzazione sequenziale dei computer, agevolando l'*accesso rapido alle righe* di una tabella, soprattutto in presenza di un elevato numero di colonne.

> [!example] <font color="orange">Esempio</font>
>Diciamo di avere un [[Dataset]] di 1.000 righe (*sample*) e che ogni riga abbia 10 colonne (*features*). Allora i formati di tipo row major come CSV sono migliori per accedere ai sample.




D'altra parte, i formati **column major** risultano più efficienti quando l'obiettivo principale è la *lettura di molte colonne*. Questi formati offrono una flessibilità di *lettura basata sulle colonne*, dimostrandosi particolarmente vantaggiosi in situazioni con dati estesi caratterizzati da migliaia o milioni di feature, ma con un numero di colonne limitato.

> [!example] <font color="orange">Esempio</font>
>Diciamo di avere un dataset di 1.000 colonne (*features*). Allora i formati di tipo column major come Parquet sono migliori per filtrare le features.

### Dati Binari e di Testo
I **File di testo** sono documenti composti da testo in chiaro, facilmente leggibili dagli esseri umani.

Contrariamente, il termine **File binari** si riferisce a tutti i documenti non testuali, contenenti sequenze di 0 e 1, concepiti per essere interpretati da programmi specifici. Questi file binari sono più *efficienti dal punto di vista della memoria*, richiedendo meno spazio per la memorizzazione.




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