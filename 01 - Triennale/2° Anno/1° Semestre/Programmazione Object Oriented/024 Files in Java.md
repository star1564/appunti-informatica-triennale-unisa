---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Caratteristiche della Programmazione ad Oggetti
cssclasses:
  - 
---
## FORMATO DEI FILES
Consideriamo come esempio il numero intero 12345.

I dati sono memorizzati nei files in due formati:
- Uno [[022 Stream|Stream]] di **Testo**: una successione di caratteri; 
	- 1, 2, 3, 4, 5
- Uno Stream di **Bytes**: una successione di bytes, ovvero in base 256;
	- 0, 0, 48, 57 (dove $12345 = 48\cdot256+57$)
	- [[003 Base Qualunque e Decimale#DECIMALE → BASE QUALUNQUE|Conversione da decimale a base qualunque (256)↩]]

Vedremo solo metodi e classi che hanno a che fare con il formato di testo.

## FILES
La classe **`File`** crea una [[001 Introduzione a OOP#^1dfc00|astrazione]] del concetto di file. Viene usato per manipolare **file esistenti**. 
Se il file non esiste, questo **NON lo creerà**.

```Java
File nome = new File(PathFile);
```

Non si può leggere o scrivere direttamente da un oggetto di tipo `File`, ma lo possiamo passare come argomento nel costruttore delle classi che vedremo più avanti.

### PATH
In generale, il **path di un file** viene specificato attraverso l'utilizzo di due barre rovesciate *`\\`*, questo perché ogni singola barra rovesciata è un carattere di escape.
```markup
"C:\\nome_directory\\nome_file.ext"
```

Se non si inserisce il path assoluto di un file, ma solo il suo nome, si ci riferrerà ad un file nella stessa cartella del `.class` (o nella cartella principale di un progetto Eclipse).
```markup
"nome_file.ext"
```

### MODIFICARE UN FILE
Alcuni [[Metodi]] per modificare un file sono:
- *`delete()`*: cancella il file restituendo `true` se la cancellazione ha avuto successo;
- *`renameTo(File newName)`*: rinomina il file restituendo `true` se la ridenominazione ha avuto successo;
- *`length()`*: restituisce la lunghezza del file in byte (0 se il file non esiste);
- *`exists()`*: restituisce `true` se il file o la directory esiste.

## LETTURA DA FILE
Per leggere da un file si usa la classe **`FileReader`**. 
Normalmente gli oggetti di questa classe leggono un carattere per volta, quindi per leggere intere linee si usa [[005 Input da Terminale|Scanner]].
```Java
File f = new File("file.txt"); /* ATTENZIONE: non crea il file */

FileReader reader = new FileReader(f); /* Si può usare anche la stringa con il path */

Scanner in = new Scanner(reader);
String line = in.nextLine();

reader.close();
```

Si usano gli stessi metodi per uno `Scanner` da tastiera.

Bisogna chiudere il flusso del file con la `close()`, altrimenti il cursore rimarrà alla posizione corrente e potrebbe essere usato da altre classi per file.

## SCRITTURA SU FILE
Per scrivere su un file si usa la classe **`PrintWriter`**. *Se il file non esiste lo crea*, anche se si utilizza con `File`.
La `close()` **rende effettivi** i cambiamenti.
```Java
PrintWriter pw = new PrintWriter("C:\\HelloWorld.txt"); /* Se il file non esiste lo crea */
pw.println("Prima riga"); /* Va a capo */
pw.print("Hello World alla fine del file"); /* Non va a capo */
pw.close(); /* Chiude e salva le modifiche */
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