---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Persistence API
cssclasses:
  - 
---
>Una **Entità** è un oggetto [[023 POJO|POJO]] che *vive in modo persistente in un database* e che viene portato in [[030 Memoria Centrale|Memoria Principale]]. L'[[035 Object Relational Mapping|ORM]] permette la manipolazione delle entità, mascherando gli accessi al database.

![[Pasted image 20231115101314.png|600]]

Queste entità, una volta mappate ad un database, possono essere gestite da [[033 Java Persistence API|JPA]] attraverso [[039 JPQL|Java Persistence Query Language]].

> [!question] Qual è la differenza tra una entità ed un oggetto?
> Una Entità differisce da un oggetto normale, perché questi ultimi *vivono solamente nella memoria principale*. Invece le Entità persistono nel database, anche se vengono eliminate dalla memoria principale dal [[014 Garbage Collector|Garbage Collector]] (in pratica "vanno e vengono" dal database).

Le operazioni che è possibile eseguire sulle entità rientrano in 4 categorie:
- *Persisting*, l'inserimento di una nuova nel database;
- *Updating*, l'aggiornameto di una riga esistente nel database;
- *Removing*, la rimozione di una riga esistente dal database;
- *Loading*, l'operazione di `SELECT` dal database.


### Anatomia di un'Entità
Un POJO diventa una entità quando:
- La classe è annotata con **`@Entity`**, che permette al persistence provider di riconoscerlo come una classe persistente;
- Si usa l'annotazione **`@Id`**, che definisce l'identificatore unico (primario) per l'entità. `@GeneratedValue` fa generare automaticamente un valore unico;
- Il costruttore pubblico non ha argomenti;
- Non è un'[[018 Interfacce|interfaccia]] o [[010 Tipo Enumerativo|enum]];
- Non è [[Keyword final|final]] e nessun metodo al suo interno è final;
- Se deve essere passata per valore (come in un metodo remoto) deve implementare l'interfaccia [[025 Serializzazione|Serializable]].

Gli attributi non annotati verranno resi persistenti applicando un mapping di default.

Un'entità *può avere anche dei metodi di business*.

```Java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
public class Book implements Serializable {
	@Id @GeneratedValue
	private Long id;
	private String title;
	private Float price;
	private String description;
	
	// Costruttore no-args
	public Book() {}
	
	// Costruttori, Getters, setters
}
```



___
[[000 Indice PD|↖ Ritorna all'indice ↖]]

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