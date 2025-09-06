---
aliases:
  - Classe Wrapper
  - Tipo Wrapper
  - Tipo Boxato
  - Tipi Boxati
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Basi della Programmazione ad Oggetti
cssclasses:
  - 
---
## BOXING
Per motivi pratici abbiamo spesso a che fare con [[003 Tipi di Dati Primitivi|tipi primitivi]] (`int`, `double`, `boolean`, ...) che non sono oggetti, ma "tipi semplici".
A volte ci conviene convertire questi tipi primitivi in degli [[Oggetti]]. Questo avviene attraverso il **Tipo Boxato** (o **Classe Wrapper**).

Un tipo boxato rende disponibile la possibilità di *usare tipi primitivi di dati sotto forma di oggetti*.
Dato un tipo primitivo, si ottiene il corrispondente Wrapper capitalizzando il nome come mostrato nella tabella seguente:

![[Pasted image 20220927121838.png|500]]

Per creare un tipo boxato si usa l'operazione di **Boxing**, cioè "inscatolamento" del tipo primitivo *nel relativo tipo wrapper* al fine di utilizzare un oggetto e tutte le sue proprietà (ad esempio porre un intero in una lista o operazioni che hanno necessità di maneggiare oggetti).

```java
Integer x = new Integer (10);
Double y = new Double (5.5);
Boolean z = Boolean.parseBoolean("true");
```

## AUTOBOXING
Attraverso l'**Autoboxing**, gli oggetti vengono automaticamente creati *con i valori di riferimento dettati*, senza generare errori. Questo permette di scrivere codice più leggibile e maneggevole.

```java
Integer x = 10;
Double y = 5.5f;
Boolean z = true;
Number n = 0.0f;
```

## UNBOXING

Alla funzione di boxing è associata anche l'operazione di **Unboxing** che trae gli stessi vantaggi della precedente.

```java
//Esempio di operazione di unboxing
int x = -1;
Integer y = x;
```

---

Tutto questo permette allo sviluppatore di non preoccuparsi delle operazioni di conversione (boxing e unboxing) lasciandole al compilatore del bytecode che si occuperà di gestirle per noi (autoboxing).

> [!example]+ <font color="orange">Esempi</font>
>```Java
>//Crea un intero boxato
>Integer myInt = 500;
>//L'intero viene convertito in stringa attraverso un metodo dell'Integer
>String myString = myInt.toString(); 
>//Stampo la lunghezza della stringa
>System.out.println(myString.length()); //3
>```
>```Java
>int val = 44;
>Integer value = new Integer(val); //Intero boxato
>int valueBack = value.intValue();
>```

## Altre Classi Boxate
- [[009 Tipo String]];
- [Classe Byte](https://www.javatpoint.com/java-byte);
- [Classe Short](https://www.javatpoint.com/java-short)
- [Classe Integer](https://www.javatpoint.com/java-integer);
- [Classe Long](https://www.javatpoint.com/java-long);
- [Classe Float](https://www.javatpoint.com/java-float);
- [Classe Double](https://www.javatpoint.com/java-double);
- [Classe Boolean](https://www.javatpoint.com/java-boolean).

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