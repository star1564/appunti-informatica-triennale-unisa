---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Introduzione da Programmazione 1
cssclasses:
  - 
---
## FLUSSI (STREAM)
In C, il termine **Stream** indica una sorgente di input o una destinazione per l'output.

Molti piccoli programmi ottengono li loro input da uno stream (ad esempio la *tastiera*) e lo inviano ad un altro stream (ad esempio il *video*.).
Programmi più grandi possono avere necessità di usare più stream.

Gli stream rappresentano file memorizzati da qualche parte (hard disk o altri tipi di memoria a lungo termine) e sono associati a periferiche (schede di rete, stampanti, ecc.).

## APRIRE E CHIUDERE I FILE
Si ha che:
- `fp` è il "file pointer" ed è il puntatore al file;
- `filename` è il nome del file da aprire;
- `mode` è una "stringa di modalità" che specifica quali operazioni abbiamo intuizione di compiere su file (read, write, ecc.).
### APRIRE
Per **aprire** i file si usa:

Prototipo:
`FILE *fopen(const char *filename, const char *mode);`

Chiamata tipica:
`FILE *fp = fopen(NOME_FILE, "mode");`

Ritorna `NULL` se il file non si apre correttamente.

### CHIUDERE
Per **chiudere** i file si usa:

Prototipo:
`int fclose(FILE *stream);`

Chiamata tipica:
`fclose(fp);`

Ritorna $0$ in caso di successo, `EOF` in caso di fallimento.

### MODALITÀ DI APERTURA (`mode`):
- `"r"` Aprire il file in lettura (il file deve esistere)
- `"w"` Aprire il file in scrittura (non è necessario che il file esista)
- `"a"` Aprire il file in accodamento (non è necessario che il file esista)
- Le versioni `+` sono in scrittura e lettura.

## I/O DA STREAM
### FGETS
Legge da stream fino a `'\n'` o fino a `size-1` e memorizza in `s`.

Prototipo:
`char *fgets(char *s, int size, FILE *stream);`

Chiamata tipica:
`fgets(string, sizeof(string), fp); //Lettura da File`
`fgets(string, sizeof(string), stdin); //Lettura dall'Input`

Ritorna `s` in caso di successo, `NULL` in caso di errore o se si raggionge la fine del file (`EOF`) senza aver letto alcun carattere.

### FSCANF
Legge da stream fino ad un carattere di spazio e non lo memorizza.

Prototipo:
`int fscanf(FILE *stream, const char *format, …); `

Chiamata tipica:
`fscanf(fp, "%[^0123456789] %d %[^\n]", nome, &area, regione);`

Dove `%[^]` legge e mette nella variabile tutto quello che trova tranne quello che si inserisce dopo il `^`.

Restituisce il numero di dati letti e scritti con successo.

### FPRINTF
Scrive su stream.

Prototipo:
`int fprintf(FILE *stream, const char *format, …); `

Chiamata tipica:
`fprintf(stderr, "Messaggio Errore Input Utente");`
`fprintf(fp, "Scrittura sul File");`

## I/O SU STRINGHE
Queste funzioni possono leggere e scrivere dati usando una stringa come se fosse un flusso.

### SSCANF
Legge i caratteri da una stringa (puntata dal primo argomento).

Prototipo:
`int sprintf(char *restrict buffer, const char *restrict format, …);`

Restituisce il numero di caratteri memorizzati.

### SPRINTF
Scrive caratteri in una stringa.

Prototipo:
`int sscanf(const char *str, const char *format, …);`

Chiamata tipica:
`sscanf(string, "%d %d", &i, &j);`

Restituisce il numero di dati letti e scritti con successo.

___
[[000 Indice PSD|↖ Ritorna all'indice ↖]]

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