---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Sincronizzazione Processi
cssclasses:
  - 
---
Un **Semaforo** è uno strumento di [[026 Sincronizzazione Processi|sincronizzazione dei processi]]. 
Questo è una *variabile intera* che si può modificare solamente attraverso due [[Operazione atomica|operazioni atomiche]], **`wait()`** e **`signal()`**.

> [!warning] Queste due funzioni non devono essere confuse con le funzioni UNIX

```C
typedef int Semaforo;

wait(Semaforo s){
	while (s <= 0) // Se s non è positivo aspetta un incremento
		;
	s--; // Decrementa s
}

signal(Semaforo s){
	s++; // Incrementa s
}
```

Mentre un processo cambia il valore del semaforo, nessun altro processo può contemporaneamente modificare quello stesso valore.
L'obiettivo della variabile semaforo è quello di *contare il numero di risorse a disposizione*, per questo lo si inizializza ad esso.

### UTILIZZO
I processi che desiderano entrare nella loro [[Sezione Critica]], invocano la `wait()` sul semaforo, decrementandone così il valore.

I processi che escono dalla loro sezione critica, invocano `signal()`, incrementando il valore del semaforo.

Questo implica che quando il semaforo vale 0, tutte le istanze della risorsa sono allocate ed i processi che le richiedono devono sospendersi sul semaforo fino a che esso diventa nuovamente positivo.

> [!example]- <font color="orange">Esempio</font>
> Abbiamo tre processi che cercano di usare la CPU. La CPU ha solo due risorse a disposizione, quindi il semaforo sarà 2.
> 1. Entrano i processi $P_1$ e $P_2$, mentre $P_3$ resta in attesa, dato che `s` è 0;
> 2. $P_2$ finisce il suo lavoro ed esegue un `signal` incrementando `s` di 1, dato che ora è un numero positivo $P_3$ entra nella CPU.
> ![[Pasted image 20221122111918.png]]

---
### SEMAFORO BINARIO
Un **Semaforo Binario (`mutex`)** può assumere come valore solo 0 oppure 1
```C
typedef int Semaforo;

Semaforo mutex = 1;

while(1){
	wait(mutex);
		/*sezione critica*/
	signal(mutex);
		/*sezione NON critica*/
}
```

---

> [!info] Più semafori
> Si può usare più di una variabile semaforo per gestire due o più processi.

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