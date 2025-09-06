---
aliases:
  - Stringa
  - Stringhe
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: 
cssclasses:
  - 
---
`String` è una classe i cui oggetti sono delle *stringhe* (un array di caratteri).

Ogni carattere ha un suo numero (indice).

| *Stringa:* | H   | e   | l   | l   | o   |     | W   | o   | r   | l   | d   |
| -------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| *Indice:*  | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  |



#### METODI STRING
| Metodo        | Restituisce         | Parametri espliciti                               | Descrizione                                                                                                                                      |
| ------------- | ------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| *toUpperCase* | rif. Oggetto String | nessuno                                           | Ritorna una stringa con tutti i caratteri MAIUSCOLI                                                                                              |
| *toLowerCase* | rif. Oggetto String | nessuno                                           | Ritorna una stringa con tutti i caratteri minuscoli                                                                                              |
| *lenght*      | intero              | nessuno                                           | Ritorna il numero di caratteri di una stringa                                                                                                    |
| *trim*        | rif. Oggetto String | nessuno                                           | Ritorna una stringa con gli spazi all'inizio ed alla fine rimossi                                                                                |
| *concat*      | rif. Oggetto String | rif. Oggetto String                               | Ritorna una stringa concatenata con un altra                                                                                                     |
| *substring*   | rif. Oggetto String | <font color="#FF6611">un intero</font>            | Ritorna una sottostringa a partire dal carattere corrispondente al numero specificato fino alla fine della stringa                               |
| *substring*   | rif. Oggetto String | <font color="#FF6611">due interi</font>           | Ritorna una sottostringa che parte dal primo numero e finisce al secondo numero - 1                                                              |
| *charAt*      | carattere           | intero                                            | Ritorna il carattere all'indice specificato                                                                                                      |
| *indexOf*     | intero              | <font color="#00C575">String/char</font>          | Ritorna la posizione della prima occorrenza di una data sottostringa in una stringa (ritorna -1 se ha esito negativo)                            |
| *indexOf*     | intero              | <font color="#00C575">String/char e intero</font> | Ritorna la posizione della prima occorrenza di una data sottostringa in una stringa a partire da una posizione (ritorna -1 se ha esito negativo) |
| *replace*     | rif. Oggetto String | *due char*                                        | Ritorna una stringa con tutte le occorrenze del primo char cambiate con quello del secondo char                                                  |
| *replace*     | rif. Oggetto String | *due rif. Oggetti String*                         | Ritorna una stringa con tutte le occorrenze della prima sottostringa cambiate con quello della seconda sottostringa                              |
| *isBlank*     | booleano            | nessuno                                           | Ritorna true se la stringa è vuota, falsi altrimenti (invece di usare `if(str = "")` usa `if(str.isBlank)`)                                                                                                                                                |
 


#### RIMPIAZZARE UN CARATTERE IN UNA STRINGA
```Java
String s = "Hello World"; //Stringa iniziale
int index = 4; //Voglio rimpiazzare la 'o'
char ch = 'e'; //Carattere con cui sostituire

s = s.substring(0, index) + /*Hell*/
	ch + /*e*/
	s.substring(index+1); /* World*/
```

___
[[000 Indice POO|↖ Ritorna all'indice ↖]]

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
- [Javapoint String](https://www.javatpoint.com/java-string)

