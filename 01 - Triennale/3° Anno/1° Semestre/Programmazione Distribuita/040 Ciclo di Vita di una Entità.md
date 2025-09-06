---
aliases: Callback entità, Listener entità
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Persistence API
cssclasses:
  - 
---
Questo [[UML 3 - Diagramma di Stato|Diagramma di Stato]] rappresenta l'intero **Ciclo di Vita di una [[034 Entità JPA|Entità]]** gestita da un [[037 Entity Manager|Entity Manager]].

![[Pasted image 20231114170253.png|800]]

Il ciclo di vita si divide in 4 stati:
- **Exists in Memory**: Quando una entità è istanziata attraverso l'operatore `new`, *è vista come un normale [[023 POJO|POJO]] dalla JVM* e può essere usata come un normale oggetto dall'applicazione.
	- In questo stato, [[033 Java Persistence API|JPA]] non è a conoscenza della sua esistenza.
- **Managed**: Quando il POJO viene preso in carico da un Entity Manager attraverso la `persist()`, viene reso una *entità persistente* e viene detta un'*Entità Managed*.  ^f8ef5d
	- In questo stato, l'Entity Manager effettuerà automaticamente la sincronizzazione del valore dell'entità con il database (ad esempio se si cambiano i suoi valori attraverso i metodi set, il nuovo valore verrà automaticamente sincronizzato con il database).
	- Si entra in questo stato automaticamente quando si carica una entità dal database attraverso la `find()`. 
- **Removed**: Dallo stato Managed è possibile *rimuovere un'entità dal database* attraverso la `remove()`. 
	- In questo stato l'entità non si trova più nel database, ma il POJO continuerà a vivere nella memoria principale (stato Exists in Memory) fino a quando non sarà eliminata dal Garbage Collector.
- **Detached**: Dallo stato Managed è possibile *far diventare detached un'entità* attraverso la `clear()` o la `detach()`.
	- In questo stato l'entità persiste nel database, ma è rimossa dal [[027 Context e Dependency Injection#^d27126|contesto]] di persistenza, ovvero viene *scollegata* da questo contesto di persistenza e non è più automaticamente gestita. Ciò significa che eventuali modifiche apportate a un'entità detached non verranno automaticamente propagate al database.
	- È possibile farla diventare di nuovo Managed attraverso la `merge()`.[^1]

[^1]: [[037 Entity Manager#^f70afc|Esempio]]

I Metodi di Callback ed i Listeners permettono di inserire della logica di business all'interno di certi eventi del ciclo di vita di una entità.

## Callbacks
Il ciclo di vita rientra (da questo punto di vista) in quattro categorie:
- *Persisting*, inserimento nel database;
- *Updating*, aggiornamento nel database;
- *Removing*, eliminazione dal database;
- *Loading*, selezione dal database.

Ognuno di questi cicli di vita hanno un [[032 Eventi (CDI)|evento]] *"pre"* e *"post"* che possono essere intercettate dall'Entity Manager per invocare un metodo di business. Questi metodi sono chiamati **Metodi di Callback**.

I metodi di callback sono inglobati all'interno della definizione della entità.

> [!tip]- Annotazioni
> I metodi di business devono essere annotati dalle seguenti annotazioni.
>![[Alcune delle Annotazioni in CDI#Callback Ciclo di Vita di un'Entità]]

![[Pasted image 20231115132855.png|600]]

> [!example]- <font color="orange">Esempio</font>
>```Java
>// All'interno dell'entità
>// ...
>@PrePersist
>@PreUpdate
>private void validate() {
>	if (firstName == null || "".equals(firstName))
>		throw new IllegalArgumentException("Invalid first name");
>	if (lastName == null || "".equals(lastName))
>		throw new IllegalArgumentException("Invalid last name");
>}
>// ...
>```

## Listeners
Nel caso in cui si voglia estrapolare la logica di un metodo di callback per applicarla a diverse entità (*condividendo il codice*), di deve definire un **Entity Listener**.

Un entity listener *è un POJO su cui è possibile definire metodi di callback*. All'interno del listener si utilizzano i metodi di callback in modo del tutto normale.

L'entità interessata provvederà a registrarsi a questi listeners usando l'annotazione **`@EntityListeners`**, passandogli la classe del listener.


> [!example]- <font color="orange">Esempio</font>
>```Java
>public class ValidationListener {
>	// ...
>	@PrePersist
>	@PreUpdate
>	private void validate() {
>		if (firstName == null || "".equals(firstName))
>			throw new IllegalArgumentException("Invalid first name");
>		if (lastName == null || "".equals(lastName))
>			throw new IllegalArgumentException("Invalid last name");
>	}
>	// ...
>}
>```
>
>```Java
>@EntityListeners({ValidationListener.class})
>@Entity
>public class Customer {
>	// ...
>}
>```

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