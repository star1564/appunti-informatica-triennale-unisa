---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Programmazione Modulare
cssclasses:
  - 
---
Un **Oracolo** è un modo per testare un programma modulare.
È composto da tre file `.txt` ed un file `driver.c`.

I tre file `.txt` sono:
- `input.txt`, dove inseriamo le combinazioni in input da testare.
	![[Pasted image 20220315123902.png]]
- `oracolo.txt`, dove ci saranno i risultati attesi.
	![[Pasted image 20220315124028.png]]
- `output.txt`, dove ci sarà il risultato del testing ($1$ = successo, $0$ fallimento).
	![[Pasted image 20220315124134.png]]

Il file `driver.c` sarà composto da un main che potremo utilizzare per testare parti di un programma. Utilizza i [[002 File in C|file]] per operare sui `.txt`.

```C
//Da compilare con: make driver
//Da aprire con: ./driver.out input.txt oracolo.txt output.txt

#include <stdio.h>
#include <stdlib.h>
#include "vettore.h"
#define N 100

int main(int argc, char *argv[]){
	FILE *fp_input, *fp_oracolo, *fp_output;
 
	if((fp_input = fopen(argv[1], "r")) == NULL){
		fprintf(stderr, "Errore apertura file di INPUT %s\n", argv[1]);
		exit(EXIT_FAILURE);
	}

	if((fp_oracolo = fopen(argv[2], "r")) == NULL){
		fprintf(stderr, "Errore apertura file ORACOLO %s\n", argv[2]);
		exit(EXIT_FAILURE);
	}

	if((fp_output = fopen(argv[3], "w")) == NULL){
		fprintf(stderr, "Errore apertura file di OUTPUT %s\n", argv[3]);
		exit(EXIT_FAILURE);
	}

	char line[N]; //Buffer per contenere i caratteri letti nel file.
	int arr_input[N]; //Conterrà una linea del file input.txt
	int arr_oracolo[N]; //Conterrà una linea del file oracolo.txt
	int n_input; //La dimensione di arr_input[].
	int n_oracolo; //La dimensione di arr_oracolo[].
	int test; //Usata per esprimere il risultato del testing: 0 = fallimento; 1 = successo.
	int i;

	for (i = 1; fgets(line, N, fp_input) != NULL; i++) {
		//La funzione fgets() legge una linea dallo stream immagazzinandola nel buffer puntato da line.
		
		n_input = convertArrayStr(arr_input, line);
		//input_array_str estrapola i dati dal buffer puntato da line.
		fgets(line, N, fp_oracolo);
		n_oracolo = input_array_str(arr_oracolo, line);


		/*FUNZIONE DA TESTARE E RELATIVA ASSEGNAZIONE A test*/
	
		fprintf(fp_output, "Test case %d: %d\n", i, test);
		//Scrive il risultato sul file output.txt
	}

	fclose(fp_input);
	fclose(fp_oracolo);
	fclose(fp_output);
	
	return 0;
}
```

### FUNZIONI NECESSARIE
- [[1010.F Convert Array Str|Convert Array Str]];
- Se dobbiamo verificare che un array sia uguale ad `arr_oracolo` si usa la funzione [[1011.F Compare Arrays|Compare Arrays]].

---
Se dobbiamo ricevere in input due array, nel file `input.txt` separiamo i due array con una virgola ed uno spazio (`, `).
Nel `driver.c` inseriamo un secondo `arr_input[]` con relativa dimensione:
```C
n_input1 = convertArrayStr(arr_input1, line);
n_input2 = convertArrayStr(arr_input2, line+(n_input1*2)); //n_input1 * 2 = dimensione array 1 + spazi.
```

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
<center><font color="rainbow"><i>"And don't worry about the vase."</i></font></center>

Altri collegamenti: 

