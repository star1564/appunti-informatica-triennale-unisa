---
aliases:
  - happens-before
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Thread
cssclasses:
  - 
---
I [[008 Thread in Java|Thread in Java]] comunicano principalmente condividendo l'accesso ai *tipi primitivi* ed ai *riferimenti a oggetti*. Proprio per questo possono avvenire *interferenze tra thread* e *inconsistenza della memoria*.

Per risolvere questi problemi, è necessaria la [[011 Metodi Sincronizzati|sincronizzazione]], ma essa può generare problemi di contesa quando più thread cercano di accedere alla stessa risorsa simultaneamente.

### Interferenze tra thread
>L'**interferenza** si verifica quando due operazioni, eseguite *in thread diversi* ma che *interessano gli stessi dati*, si *sovrappongono*. 

> [!example]- <font color="orange">Esempio</font>
>Considerare una classe `Counter` che incrementa o decrementa il valore di un intero.
>
>Se più thread *accedono contemporaneamente* a un oggetto Counter, potrebbero verificarsi problemi dovuti all'interferenza tra questi thread.
>
>Anche istruzioni apparentemente semplici come `c++` possono essere suddivise in più passaggi (stessi passaggi all'interno di un [[056 Data Path del Processore MIPS|processore]]), tra cui:
>1. Recupera il valore corrente di `c`;
>2. Aumentare il valore recuperato di 1;
>3. Memorizzare nuovamente il valore incrementato in `c`.
>
>Allo stesso modo, `c--` segue questi passaggi ma diminuisce invece il valore.
>
>Se il thread A chiama `incremento()` mentre il thread B chiama `decremento()` quando `c` è inizialmente 0, abbiamo una situazione del genere:
>
>1. Thread A: Recupera `c`;
>2. Thread B: Recupera `c`;
>3. Thread A: aumenta il valore recuperato (risultante in 1);
>4. Thread B: diminuisce il valore recuperato (risultante in -1);
>5. Thread A: memorizza il risultato in `c`, rendendo `c` uguale a 1;
>6. Thread B: memorizza il risultato in `c`, rendendo `c` uguale a -1;
>
>Il risultato del thread A viene *sovrascritto* dal risultato del thread B. Questo è solo uno scenario possibile e il risultato può variare.

### Inconsistenza della memoria
>Gli errori di **inconsistenza della memoria** si verificano quando *thread diversi hanno visualizzazioni incoerenti degli stessi dati*. 

Per evitare errori di inconsistenza della memoria, è fondamentale comprendere la relazione **happens-before** (succede prima). Questa relazione garantisce che le scritture in memoria effettuate da un'istruzione *diventino visibili* a un'altra istruzione specifica. ^4112c2

> [!example]- <font color="orange">Esempio</font>
>C'è un campo intero, `contatore`, condiviso tra i thread A e B. Il thread A incrementa `contatore` con `contatore++` e poco dopo il thread B stampa `contatore` con `System.out.println(contatore)`.
>
>Se entrambe le istruzioni fossero eseguite nello stesso thread, potremmo tranquillamente supporre che il valore stampato sarebbe "1". Tuttavia, se queste istruzioni vengono eseguite in thread separati, il valore stampato potrebbe essere ancora "0" perché non vi è alcuna garanzia che la modifica del thread A in `contatore` sarà visibile al thread B, a meno che il programmatore non stabilisca una relazione happens-before tra loro.

Inoltre, accade che:
1. Quando un'istruzione richiama `thread.start()`, qualsiasi istruzione con una relazione happens-before con quell'istruzione *ne ha anche una con ogni istruzione eseguita dal nuovo thread*. Ciò garantisce che gli effetti del codice che porta alla creazione del nuovo thread siano visibili a quel thread;
2. Quando un thread termina e viene restituito un `thread.join()` in un altro thread, tutte le istruzioni eseguite dal thread terminato stabiliscono una relazione happens-before *con tutte le istruzioni successive al join riuscito*. Ciò significa che gli effetti del codice nel thread terminato sono ora visibili al thread che esegue l'unione.


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
- [Interferenza - Java Doc](https://docs.oracle.com/javase/tutorial/essential/concurrency/interfere.html)
- [Inconsistenza - Java Doc](https://docs.oracle.com/javase/tutorial/essential/concurrency/memconsist.html)