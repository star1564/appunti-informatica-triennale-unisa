---
aliases:
  - OCL
  - OCL Set
  - OCL Sequence
  - OCL Bag
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - Object Design
cssclasses:
  - 
---
>L'**Object Constraint Language** (*OCL*) è un linguaggio che consente di *specificare formalmente i vincoli su singoli elementi del modello* (ad esempio, attributi, operazioni, classi) o su gruppi di elementi del modello (ad esempio, associazioni e classi partecipanti). Utilizza i tre tipi di [[Contratti]].

Un vincolo è espresso come un'espressione booleana che restituisce il valore Vero o Falso. Un vincolo può essere rappresentato come una nota collegata all'elemento UML vincolato da una relazione di dipendenza.

![[Pasted image 20240121082105.png|800]]

Dato che nel diagramma creano disordine, le espressioni OCL possono espresse in forma testuale.

```markup
context ClassName::methodName(par:ParamType) contract:
	condition()
```

Dove:
- `context`: indica l'[[014 Modello ad Oggetti#^7cb8de|Entity]] a quale si applica l'espressione.
- `ClassName`: è il nome della entity a cui si applica l'espressione.
	- `::methodName(par:ParamType)`: utilizzato solamente per precondizione e postcondizione, è il nome con parametri del metodo dell'entità a cui si applica l'espressione.
- `contract`: può essere uno dei tre tipi di contratti come `inv` per [[Contratti#^fba292|Invariante]], `pre` per [[Contratti#^e78e08|Precondizione]] e `post` per [[Contratti#^438049|Postcondizione]].
- `self`: denota che tutte le istanze della classe devono avere questo contratto e che devono seguire la condizione.
- `condition`: la condizione che deve essere verificata. È possibile usare nomi di altri metodi.

Nelle espressioni di *postcondizione* è possibile indicare il valore restituito da un'operazione oppure il valore di un attributo *prima della chiamata* attraverso `@pre.nomeMetodo()` o `@pre.nomeAttributo`.

> [!example]- <font color="orange">Esempio</font>
>Questo indica che non è possibile avere una dimensione della tabella negativa. Questa condizione deve essere sempre vera.
>```markup
>context HashTable inv:
>	self.size >= 0
>```
>
>Questo indica che non si può inserire una coppia chiave valore se già esiste una chiave nella tabella. Questa condizione deve essere controllata prima dell'inizio del metodo.
>```
>context HashTable::put(key:Key, value:Object) pre:
>	!containsKey(key)
>```
>
>Questo indica che dopo la fine del metodo, la dimensione della tabella deve essere la dimensione precedente più uno. Questa condizione deve essere controllata dopo la fine del metodo.
>```
>context HashTable::put(key:Key, value:Object) post:
>	getSize() = @pre.getSize() + 1
>```

## Tipi di insiemi in OCL
In generale, i vincoli coinvolgono un certo numero di classi ed attributi. Si parte *dalle classi di interesse* per poi *navigare verso le altre classi* nel modello.
Si distinguono tre tipi di navigazione:
- **Attributo Locale**: il vincolo coinvolge un attributo che è locale alla classe di interesse.
- **Classe Direttamente Relazionata**: l'espressione coinvolge la navigazione dalla classe di interesse verso un'altra classe attraverso una relazione.
- **Classe Indirettamente Relazionata**: il vincolo coinvolge la navigazione tra una serie di associazioni verso una classe indirettamente relazionata.

Si esprime in un diagramma attraverso una freccia.

![[Pasted image 20240121090253.png|700]]

Quando si lavora con [[UML 5 - Diagramma di Classi#^7f05ca|associazioni molti a molti]], OCL fornisce tre tipi di **collezioni**.

Per accedervi, esistono delle operazioni standard utilizzabili con la sintassi:

```markup
collection->operation()
```

### OCL Set
È un insieme *non ordinato* di oggetti che si esprime con `{elemento1, elemento2, elementoN}`.

Sono usati quando si naviga una *singola associazione*. Da notare che, se l'associazione ha molteplicità 1, la navigazione farà ottenere direttamente un oggetto e non un set.

### OCL Sequence
È una sequenza *ordinata* di oggetti che si esprime con `[elemento1, elemento2, elementoN]`.

Sono usati quando si naviga una *singola associazione ordinata*.

### OCL Bag
È un *multi-insieme*, ovvero una serie di insiemi usati per accumulare oggetti quando is accede ad oggetti correlati in modo *indiretto*.
Possono essere presenti oggetti ripetuti e l'insieme vuoto.

---
> [!tip] Euristiche per scrivere vincoli leggibili
>- Concentrarsi sulla vita di una classe: i vincoli che sono specifici per le operazioni o che valgono solo quando l'oggetto si trova in determinati stati sono meglio espressi come pre e postcondizioni.
>- Identificare i valori speciali per ogni attributo: i valori zero e nulli, gli attributi che hanno valori unici all'interno di un certo ambito e gli attributi che dipendono da altri attributi sono spesso fonte di malintesi ed errori.
>- Identificare i casi speciali per le associazioni: identificare, in particolare, tutti i casi speciali che non possono essere specificati con la sola molteplicità.
>- Identificare l'ordine delle operazioni.
>- Evitare vincoli che comportino molti attraversamenti di associazioni: questo spesso aumenta l'accoppiamento tra insiemi di classi non correlate, il che non è consigliabile.

___
[[000 Indice IS|↖ Ritorna all'indice ↖]]

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