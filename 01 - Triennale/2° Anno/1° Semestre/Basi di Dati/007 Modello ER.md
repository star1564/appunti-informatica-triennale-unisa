---
aliases:
  - ER
  - Entity-Relationship
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Modello Entity-Relationship
cssclasses:
  - 
---
Il **Modello Entity-Relationship** (ER) è un diffusissimo [[002 Modelli di Dati|data model]] di alto livello, usato per definire lo schema concettuale di un database.

Il modello ER descrive i dati (ER) con tre concetti fondamentali:
1. [[ER1_Entità|Entità]];
2. [[ER3_Attributo|Attributi]];
3. [[ER6_Relazione tra Entità|Relazioni]].

---
### DIAGRAMMI
#### ENTITÀ
Ogni entità viene rappresentata graficamente come un **Rettangolo**.

![[Pasted image 20221008124326.png]]

Un [[ER9_Entità e Relazioni Deboli#^b5cc34|Entità Debole]] viene rappresentata graficamente come un **Doppio Rettangolo**.
![[Pasted image 20221012174928.png|200]]

---
#### ATTRIBUTI
Ogni attributo viene rappresentato graficamente da un **Ellisse** e viene collegato a delle entità.

![[Pasted image 20221008124631.png]]

Gli [[ER4_Tipi di Attributi#^b63ded|attributi composti]] si dividono in una struttura simile a quella di un albero, vengono *collegati agli attributi padri*.

![[Pasted image 20221008124800.png]]

Gli [[ER4_Tipi di Attributi#^abce31|attributi multi-valued]] sono rappresentati graficamente con una **Doppia Ellisse**.

![[Pasted image 20221008124924.png]]

Gli [[ER4_Tipi di Attributi#^cab5d8|attributi derivati]] sono rappresentati graficamente con un'**Ellisse Tratteggiata**.

![[Pasted image 20221008125039.png]]

Gli [[ER5_ Attributi Chiave|attributi chiave]] sono rappresentati graficamente in un Ellisse con il **Nome Sottolineato**.

![[Immagine 2022-10-18 074031 1.png]]

---
#### RELAZIONI
Ogni relazione è rappresentata graficamente da un **Rombo**.

![[Pasted image 20221008123335.png|500]]

Un [[ER9_Entità e Relazioni Deboli#^3496ac|Relazione Debole]] viene rappresentata graficamente come un **Doppio Rombo**.

![[Pasted image 20221012175100.png|200]]

È possibile assegnare ad una Relazione un Attributo.

![[Pasted image 20221116090317.png|500]]

##### CARDINALITÀ
Al di sopra delle linee si scrivono i numeri del [[ER8_Rapporto di Cardinalità|Rapporto di cardinalità]]:

- [[ER7_Tipi Relazioni#^220e06|Relazioni uno ad uno]]: Quando solo un'istanza di un'entità è associata alla relazione, questa è contrassegnata come "*1:1*". L'immagine seguente riflette che solo un'istanza di ciascuna entità deve essere associata alla relazione. Rappresenta una relazione uno-a-uno. ![[Pasted image 20221008130009.png]]
- [[ER7_Tipi Relazioni#^4ad5e9|Relazioni uno a molti]]: Quando più di un'istanza di un'entità è associata a una relazione, questa viene contrassegnata come "*1:N*". L'immagine seguente riflette che solo un'istanza di entità a sinistra e più di un'istanza di entità a destra possono essere associate alla relazione. Si tratta di una relazione uno-a-molti. ![[Pasted image 20221008130107.png]]
- [[ER7_Tipi Relazioni#^27ba3d|Relazione molti a uno]]: Quando più di un'istanza di entità è associata alla relazione, questa viene contrassegnata come "*N:1*". L'immagine seguente riflette che più di un'istanza di un'entità a sinistra e una sola istanza di un'entità a destra possono essere associate alla relazione. Si tratta di una relazione molti-a-uno. ![[Pasted image 20221008130459.png]]
- [[ER7_Tipi Relazioni#^365d02|Relazione molti a molti]]: L'immagine seguente riflette che più di un'istanza di un'entità a sinistra e più di un'istanza di un'entità a destra possono essere associate alla relazione. Si tratta di una relazione molti-a-molti. ![[Pasted image 20221008130513.png]]

##### [[ER8_Rapporto di Cardinalità|Rapporti e vincoli di cardinalità]]

---
#### VINCOLI DI PARTECIPAZIONE
Si ha una:
- **Partecipazione Totale**: ogni entità partecipa alla relazione. Viene rappresentata graficamente con doppie linee;
- **Partecipazione Parziale**: non tutte le entità partecipano alla relazione. Viene rappresentata graficamente con una linea.

![[Pasted image 20221008150636.png]]


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
- [Tutorials Point](https://www.tutorialspoint.com/dbms/er_diagram_representation.htm)

