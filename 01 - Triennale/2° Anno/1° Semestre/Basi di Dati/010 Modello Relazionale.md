---
aliases:
  - database relazionale
cssclasses:
  - 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Relazionale
---

## MODELLO DI DATI RELAZIONALE
Il **Modello Relazionale** è un modello di dati logico che si basa sul [[014 Relazione|concetto matematico di Relazione]].
Queste relazioni vengono rappresentate attraverso le **Tabelle**. ^tabelle

![[Pasted image 20221126121212.png|750]]

<center>Il database relazionale è una collezione di relazioni (tabelle)</center>

Ogni relazione (tabella) ha:
- Un *nome*, che rappresenta quali entità verranno contenute all'interno di una tabella;

- Un qualsiasi numero di **Colonne**, ovvero gli [[ER3_Attributo|Attributi]] (o *Domini*); ^Colonne

- Un qualsiasi numero di **Righe**, anche chiamate **Tuple**. Ogni Tuple rappresenta una sola [[ER1_Entità|Entità]] reale; ^Righe

- Dei **Campi**, i dati inseriti all'interno di una riga. ^Campo

![[26be4b96-a086-4975-8b80-7fc185e33d85.jpg]]

In questo esempio, il nome della relazione è `Student`.

I domini sono:
- `Roll_No.` (Matricola *`Studente`*);
- `Name` (Nome *`Studente`*);
- `Department` (Dipartimento *`Studente`*).

Dato che la relazione si chiama `Student`, le tuple rappresenteranno vari studenti:
- Stevie, con matricola numero 101, del dipartimento di Computer Science;
- Jhonson, con matricola numero 265, del dipartimento di Finance;
- Margret, con matricola numero 505, del dipartimento di Biology;
- ...

> [!tip]+ Schemi ed istanze di Relazione
> Uno [[003 Schema e Stato di un database|schema di una relazione]] rappresenta solamente il nome della relazione ed i nomi dei domini. 
> Un'*istanza di relazione* rappresenta tutti i campi inseriti all'interno di quella relazione.
>![[Pasted image 20221126123403.png|600]]

^istanzaRelazione

---
### CARATTERISTICHE DI UNA RELAZIONE
Dato che ci saranno molte relazioni all'interno del database, ci devono essere alcune caratteristiche importanti.
1. Ogni Relazione nel database deve avere *nomi distinti* per poter distinguerle l'uno dall'altre;
2. Una relazione *NON deve avere due attributi con lo stesso nome*. Ogni attributo deve avere un nome distinto;
3. Le tuple *NON possono essere duplicate*;![[Pasted image 20221126123826.png|355]]
4. Ogni tuple deve avere esattamente *un solo valore per attributo* nei campi. Per esempio non si possono combinare due studenti con lo stesso dipartimento;![[Pasted image 20221126124021.png|500]]
5. Le tuple e gli attributi in una relazione *non seguono un ordine specifico*.

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
- [BinaryTerms](https://binaryterms.com/relational-data-model.html)