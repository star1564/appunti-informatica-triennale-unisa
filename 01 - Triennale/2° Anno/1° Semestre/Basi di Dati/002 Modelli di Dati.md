---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modelli di Dati
cssclasses:
  - 
---
Un **Modello di Dati** è insieme di concetti che *descrive la struttura di un database*, organizza i dati e formalizza come questi sono relazionati ad entità reali.

Per struttura di un database si intendono:
- I tipi di dati;
- Le relazioni tra i dati;
- I vincoli semantici sui dati.

L'approccio con i database ha la caratteristica fondamentale di fornire una forte [[Astrazione sui Dati]], nascondendo tutti i dettagli di memorizzazione non necessari agli utenti.

L'astrazione dei dati si ottiene per mezzo di un modello di dati. Questa astrazione è importante, perché permette di comunicare ed elaborare una *rappresentazione* del [[Mini-Mondo|miniworld]].

![[Pasted image 20221001102050.png|600]]

## CATEGORIE DI MODELLI DEI DATI
Ci sono tre tipi principali di modelli di dati: **Concettuale, Logico e Fisico**.
Ognuno di questi ha diverse *caratteristiche* (riassunte nella tabella) che vengono e non vengono mostrate.

| Caratteristica             | Concettuale | Logico | Fisico |
| -------------------------- | ----------- | ------ | ------ |
| Nomi Entità                | ✓           | ✓      |        |
| Relazioni Entità           | ✓           | ✓      |        |
| Attributi                  |             | ✓      |        |
| Chiavi Principali          |             | ✓      | ✓      |
| Foreign Keys               |             | ✓      | ✓      |
| Nomi Tabelle               |             |        | ✓      |
| Nomi Colonne               |             |        | ✓      |
| Tipi di Dati nelle Colonne |             |        | ✓      |

### MODELLI CONCETTUALI (AD ALTO LIVELLO)
I **Modelli Concettuali** forniscono concetti *semplici* su come sono organizzati i dati. 

Le caratteristiche fondamentali di questo modello sono:
- L'inclusione delle [[ER1_Entità|Entità]] più importanti e delle [[ER6_Relazione tra Entità|Relazioni]] tra esse;
- Nessuna specifica degli [[ER3_Attributo|Attributi]];
- Nessun [[ER5_ Attributi Chiave|elemento chiave]] specificato.


![[59a41bca-52c5-4aa2-a157-7e93893f73a3.jpg]]

### MODELLI LOGICI (RAPPRESENTATIVI / IMPLEMENTATIVI)
I **Modelli Logici** descrivono *con più dettagli i dati*, tralasciando come questi sono fisicamente organizzati.

Le caratteristiche fondamentali di questo modello sono:
- L'inclusione di tutte le entità e delle Relazioni tra esse;
- Tutti gli attributi sono specificati;
- Tutti gli elementi chiave sono specificati;
- Sono specificate le [[011 Vincoli del Modello Relazionale#VINCOLI DI INTEGRITÀ REFERENZIALE|Foreign keys]];
- Avviene la [[021 Forme Normali|Normalizzazione]].

![[e2b74342-4b3c-4946-bd7e-3a674a0da870.jpg]]

### MODELLI FISICI (BASSO LIVELLO)
I **Modelli Fisici** forniscono concetti che descrivono i *dettagli sul modo in cui i dati sono memorizzati* nel calcolatore, generalmente sono destinati a specialisti dei sistemi di elaborazione.

Le caratteristiche fondamentali di questo modello sono:
- La specificazione di tutte le [[010 Modello Relazionale#^tabelle|Tabelle]] e [[010 Modello Relazionale#^Colonne|Colonne]];
- Le Foreign keys sono usate per identificare le relazioni tra le tabelle;
- La de-normalizzazione può accadere se richiesta;
- Considerando le caratteristiche fisiche potrebbe rendere il modello diverso da uno logico;
- Il modello fisico cambia in base al [[DBMS]] in uso (come MySQL ed SQL Server).

![[5fc312b3-f84d-45a9-99dd-2da13d2f1f57.jpg]]





___
[[000 Indice BD|↖ Ritorna all'indice ↖]]

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