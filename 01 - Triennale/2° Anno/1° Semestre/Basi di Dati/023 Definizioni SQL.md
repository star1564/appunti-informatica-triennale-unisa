---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: SQL
cssclasses:
  - 
---
L'**SQL** (Structured Query Language) è un linguaggio *dichiarativo* basato in parte sull'[[014 Algebra Relazionale|algebra relazionale]] ed in parte sul calcolo relazionale. 
È lo standard in tutti i [[DBMS]] relazionali. Attraverso esso, l'utente specifica quale risultato deve essere raggiunto.

Permette la *definizione*, la *manipolazione* (aggiornamento e recupero) e la *gestione* di basi di dati [[010 Modello Relazionale|relazionali]], fornendo statement appropriati.
SQL può anche essere usato interattivamente o incorporato in programmi C, Java, ecc.

---
### TERMINOLOGIA SQL
SQL usa una terminologia diversa da quella classica relazionale:
- [[010 Modello Relazionale#^tabelle|Relazione]] si dice **Tabella**;
- [[010 Modello Relazionale#^Righe|Tupla]] si dice **Riga**;
- [[010 Modello Relazionale#^Colonne|Attributo]] si dice **Colonna**.

---
### SCHEMA SQL
Il concetto di **schema** SQL è usato per raggruppare tabelle ed altri costrutti che appartengono alla stessa applicazione di database.

Questo è identificato da un *nome dello schema*, ed include un identificatore di autorizzazione per indicare l'utente proprietario dello schema, così come dei *descrittori* per ogni elemento dello schema.

Uno schema include:
- Tabelle;
- Domini;
- Viste;
- Altri costrutti (come i permessi di autorizzazione, ecc.).

---
### TIPI DI DATI E DOMINI
I principali tipi di dati disponibili per gli attributi includono:
- **Tipi Numerici**:
	- *Interi* di diverse dimensioni (`INTEGER` o `INT`, `SMALLINT`);
	- *Reali in [[011 Virgola Mobile|virgola mobile]]* con diversa precisione (`FLOAT` o `REAL`, `DOUBLE PRECISION`);
	- *Numeri Formattati* (`DECIMAL(i,j)` o `DEC(i,j)` o `NUMERIC(i,j)`); 
		- `i`, detta precisione, è il numero totale di cifre decimali; 
		- `j`, detta scala, è il numero di cifre dopo la virgola.

- **Stringhe di Caratteri**, dove `n` è il numero massimo di caratteri:
	- A *lunghezza fissa* (`CHAR(n)` o `CHARACTER(n)`);
	- A *lunghezza variabile* (`VARCHAR(n)` o `CHAR VARYNG(n)`).

- **Stringhe di Bit**, dove `n` è il numero massimo di bit:
	- A *lunghezza fissa* (`BIT(n)`);
	- A *lunghezza variabile* (`BIT VARYING(n)`).

- **DATE**, ha dieci posizioni e i suoi componenti sono `YEAR`, `MONTH`, e `DAY` nella forma YYYY-MM-DD.

- **TIME**, ha almeno otto posizioni e i suoi componenti sono `HOUR`, `MINUTE` e `SECOND` nella forma HH:MM:SS.

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