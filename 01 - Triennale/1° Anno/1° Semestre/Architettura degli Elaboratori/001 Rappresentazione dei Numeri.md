---
aliases:
tags:
  - corsi/informatica/architettura_degli_elaboratori
paragrafo: Rappresentazione Numeri - Interi
cssclasses:
  -
---

Un moderno elaboratore è un *sistema elettronico digitale programmabile*: può eseguire un insieme di istruzioni (*programma*) applicandole a dei dati.  

I dati all'interno della macchina sono sempre rappresentati da **bit** (*binary digit*), cioè valori binari che possono assumere due soli stati: `0` o `1`.

## La base di un numero

La base di un numero si determina secondo quanti simboli si trovano nell'alfabeto che usiamo per rappresentarlo.

Ci sono infinite basi, ma quelle che useremo saranno:

- **Base 2**, anche chiamata **Binario** o **Binario Puro**: un alfabeto costituito da due soli simboli `{0,1}`, usata da un elaboratore per codificare, memorizzare e ricevere istruzioni
- **Base 8**: costituita dai simboli `{0, 1, 2, 3, 4, 5, 6, 7}`
- **Base 10**: la base che usiamo comunemente, che è costituita dai simboli `{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}`
- **Base 16**: costituita dai simboli `{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F}`, dove le lettere rappresentano in ordine `{10, 11, 12, 13, 14, 15}`

## Rappresentazione e conversione di un numero

In binario si può fare la *rappresentazione e la conversione*, attraverso i bit, di:

- [[002 Interi|Numeri Interi]]
- Numeri con segno:
  - [[007 Modulo e Segno|Modulo e Segno]]
  - [[008 Complemento a 2|Complemento a 2]]
- Numeri con virgola:
  - [[009 Virgola Fissa|Virgola Fissa]]
  - [[011 Virgola Mobile|Virgola Mobile]], utilizza la [[010 Notazione Scientifica|Notazione Scientifica]]
- [[012 Caratteri non numerici|Caratteri Non Numerici (ASCII)]]
- [[046 Traduzione di Istruzioni|Istruzioni e Programmi]]

___

[[000 Indice AdE|↖ Ritorna all'indice ↖]]

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
