---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Caratteristiche della Programmazione ad Oggetti
cssclasses:
  - 
---
La **Serializzazione** consiste nell'[[018 Interfacce|Interfaccia]] **`Serializable`**. Questa non ha metodi e la si fa implementare alle [[Classi]] che vogliono serializzare (o **De-Serializzare**) le loro variabili.

- La serializzazione *converte lo stato di un [[Oggetti|Oggetto]] in uno [[022 Stream|Stream]] di Bytes*;
	- È eseguita attraverso `FileOutputStream` e `ObjectOutputStream`.
- La de-serializzazione *è il processo inverso*, dove *uno Stream di Bytes è convertito (con un Cast) in un oggetto*.
	- È eseguita attraverso `FileInputStream` e `ObjectInputStream`.

![[Pasted image 20221130091431.png|450]]

> [!example]+ <font color="orange">Esempio</font>
>```Java
>import java.io.*;
>
>class ClasseGenerica implements Serializable{
>	int i;
>	String s;
>	
>	public ClasseGenerica(int i, String s) {
>		this.i = i;
>		this.s = s;
>	}
>}
>
>public class Main {
>	public static void main(String[] args) throws IOException, ClassNotFoundException {
>		ClasseGenerica c = new ClasseGenerica(20, "Test");
>		
>		// Serializzazione di c (Salvataggio Oggetto)
>		FileOutputStream fos = new FileOutputStream("foo.txt");
>		ObjectOutputStream oos = new ObjectOutputStream(fos);
>		oos.writeObject(c);
>		oos.close(); // Chiusura Stream
>		
>		// De-serializzazione di c (Caricamento Oggetto)
>		FileInputStream fis = new FileInputStream("foo.txt");
>		ObjectInputStream ois = new ObjectInputStream(fis);
>		ClasseGenerica altra = (ClasseGenerica) ois.readObject();
>		ois.close(); // Chiusura Stream
>		
>		System.out.println("altra: "altra.i + " " + altra.s);
>	}
>}
>```
>Lo stato delle variabili di quell'oggetto verranno salvate in modo non leggibile ad una persona. Per esempio:
>```markup
>¬í sr ClasseGenericaaã6Ž¾°´ž I iL st Ljava/lang/String;xp   t Test
>```
>Ma una volta eseguita la de-serializzazione dal file, viene eseguito correttamente il caricamento:
>```markup
>altra: 20 Test
>```




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