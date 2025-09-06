---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Programmazione Modulare
cssclasses:
  - 
---
Il **Makefile** permette di eseguire la compilazione ed il collegamento di moduli.

Deve essere chiamato `makefile.txt`, per poi cancellare l'estensione. Diventerà un file apribile con un editor di testo.

Si creeranno i file oggetto (file `.o`) e si eseguirà il loro *collegamento* (`link`). 

Si inserisce anche una utile funzione di pulizia (`clean`) che elimina i file che non ci serviranno nel caso dobbiamo ricompilare i file.

```
link: n_moduli.o main.o
	gcc n_moduli.o main.o -o eseguibile.out

n_moduli.o:
	gcc -c n_moduli.c

main.o:
	gcc -c main.c

clean:
	rm -f n_moduli.o main.o eseguibile.out
```

Per compilare il programma modulare si scrive nel terminale:
```
>make
```

Per eliminare i file oggetto ed esecutibili si scrive nel terminale:
```
>make clean
```

Si possono eliminare tutti i file con una data estensione in una cartella specificando: 
```
clean:
	rm -f *.o *.out
```


<font color="orange">Esempio</font>:
![[b6f9fd0f-37ef-45fe-a677-f9eb65b82512.png]]
<font color=CC241D>Controlla se ci sono i file .o necessari</font>, <font color=61AFEF>se non ci sono li crea</font>;
<font color="green">Creazione file .out</font>;
<font color="yellow">Se non servono più i file, con "make clean" si potranno cancellare</font>.

## COMPILARE UN ALTRO LINK
Si può scegliere di compilare tra più di un link. 
Il primo `link` è l'opzione di default quando si scrive

```
>make
```

![[Pasted image 20220315123130.png|700]]

Se si scrive
```
>make driver
```
Il makefile prenderà in considerazione il link con il nome del secondo parametro inserito (`driver`).

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