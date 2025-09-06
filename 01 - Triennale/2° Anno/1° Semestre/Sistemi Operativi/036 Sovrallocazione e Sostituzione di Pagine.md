---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Memoria
cssclasses:
  - 
---
Se durante l'esecuzione di un processo utente si verifica un page fault ed il sistema operativo scopre che *non ci sono frame liberi*, si ha una **Sovrallocazione**. In questo caso tutta la memoria è in uso.

Per risolvere questo problema, la maggior parte dei sistemi operativi utilizza la **Sostituzione delle Pagine**.

![[IMG_20221216_115938.jpg|600]]

<center>Necessità di sostituire le pagine</center>

Questa segue il seguente criterio: se nessun frame è libero, *ne viene liberato uno attualmente inutilizzato* in memoria spostandolo nel disco.

Si modifica la procedura di servizio dell'eccezione di page fault in modo da includere la sostituzione della pagina (insieme all'algoritmo per la selezione del *frame vittima*).

![[Pasted image 20221216120223.png|600]]

<center>Sostituzione delle pagine</center>

## ALGORITMI DI SOSTITUZIONE DELLE PAGINE
Per realizzare la sostituzione del frame vittima occorre scegliere un **Algoritmo di Sostituzione delle Pagine**.

Un algoritmo si valuta effettuandone l'esecuzione su una particolare *successione dei riferimenti* alla memoria e calcolando il numero di page fault.
La successione dei riferimenti che useremo è $$7, 0, 1, 2, 0, 3, 0, 4, 2, 3, 0, 3, 2, 1, 2, 0, 1, 7, 0, 1$$

---
### SOSTITUZIONE FIRST IN FIRST OUT (FIFO)
Questo è l'algoritmo di sostituzione più semplice. Associa a ogni pagina l'istante di tempo in cui quella pagina è stata portata in memoria.
Se si deve sostituire una pagina, *si seleziona quella presente in memoria da più tempo*.

![[Pasted image 20221216123919.png]]

<center>Algoritmo sulla successione dei riferimenti di esempio</center>

- Al 1° e 2° passo si inseriscono i valori 7, 0, 1 negli spazi vuoti;
- al 3° si deve inserire il valore 2, ma si ha page fault dato che non abbiamo altri spazi vuoti, quindi si effettua la sostituzione notando quale valore è stato di più nelle pagine. (7 per 3 volte, 0 per 2 volte, 1 per una volta), si sceglie il 7 e lo sostituisce con il valore 2;
- al 4° si deve inserire il valore 0, ma è già presente, quindi si lasciano le pagine come si trovano;
- al 5° si deve inserire il valore 3, si nota che il valore 0 è stato di più (4 volte), quindi si sostituisce con il valore 3;
- al 6° si deve inserire il valore 0, si nota che il valore 1 è stato di più (4 volte), quindi si sostituisce con il valore 0;
- ecc...

#### ANOMALIA DI BELADY
Aumentando il numero di frames is hanno più page faults per ogni successione di riferimenti.

Successione $1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5$.

Con 3 frame si hanno *9 page faults*:
![[Pasted image 20221216130911.png|450]]
Con 4 frame si hanno *10 page faults*:
![[Pasted image 20221216130950.png|450]]

---
### SOSTITUZIONE OTTIMALE DELLE PAGINE
Tale algoritmo è quello che fra tutti gli algoritmi presenta il tasso minimo di page fault e non presenta l'anomalia di Belady.
Consiste nel *sostituire la pagina che* **non si userà** *per il periodo di tempo più lungo*.

Questo richiede la conoscenza della sequenza dei riferimenti futuri.

![[Immagine 2022-12-16 131359.png]]

- All'inizio si inseriscono 7, 0 e 1.
- Al 3° si deve inserire 2, quindi si cerca quale elemento verrà utilizzato di meno nell'intera successione dei riferimenti (7 sarà usato 1 volta, 0 sarà usato 5 volte, 1 verrà usato 3 volte). Si sostituisce a 7 il valore 2.
- ecc..

---
### SOSTITUZIONE LRU (LAST RECENTLY USED)
Si sostituisce la pagina che *non è stata usata per il periodo di tempo più lungo*.

![[Immagine 2022-12-16 132834.png]]

...
Al 10° devo inserire 0, quindi cerco nella successione dei riferimenti (che inizia da 7 e finisce al 10° elemento (3)) quale valore tra 4, 3 e 2 si trova più lontano. Scopro che il 4 è più lontano quindi inserisco lo 0 al suo posto.

![[Pasted image 20221216134506.png]]

#### IMPLEMENTAZIONE LRU
Esistono diverse tecniche per implementare quest'algoritmo:
- **Contatore**:
	- Si assegna alla CPU un contatore che si incrementa ad ogni riferimento alla memoria;
	- Si assegna ad ogni entry della tabella delle pagine un ulteriore campo;
	- Ogni volta che si fa riferimento ad una pagine viene copiato il valore del contatore della CPU nel campo corrispondente per quella pagina nella tabella delle pagine;
	- Quando una pagina deve essere sostituita si guardano i valori dei contatoti, scegliendo la pagina con il valore del contatore più basso.
- **Stack**:
	- Mantenere uno stack dei numeri di pagina in una lista a doppio collegamento;
	- Riferimento ad una pagina: se è una pagina non presente nei frame la si mette in cima allo stack, ogni volta che si fa riferimento ad una pagina già presente nello stack bisogna prelevare tale riferimento dalla sua posizione e metterla in cima, nessuna ricerca per la sostituzione: basta prelevare la pagina dal fondo;
	- Richiede di cambiare al più 6 puntatori.

Le implementazioni con contatore e stack richiedono molte risorse, una soluzione più semplice è quella di assegnare alla page table, oltre al bit di validità e modifica, un **bit di riferimento** che inizialmente è 0 quando la pagina viene caricata la prima volta in memoria, quando viene fatto riferimento nuovamente a questa pagina, viene messo il bit ad 1 e nel momento in cui si deve scegliere una pagina da eliminare si va a scegliere la pagina che ha questo bit di riferimento a 0, perché significa che oltre al momento iniziale che si setta a 0, tale pagina non è stata proprio usata.

---
### ALGORITMO DELLA SECONDA CHANCE
Le pagine vengono predisposte in una sorta di lista circolare, e quando si seleziona una pagina vittima si inizia la scansione della lista.
Si parte da una certa posizione, quando si cerca una pagina vittima si cerca la pagina che ha il bit di riferimento ad 1, lo si fa diventare 0 (seconda chance) e si prosegue con la scansione, così con tutti gli altri bit ad 1.
Alla prima pagina che ha bit 0, questa sarà la vittima.

![[Pasted image 20221216135254.png|650]]

___
[[000 Indice SO|↖ Ritorna all'indice ↖]]

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