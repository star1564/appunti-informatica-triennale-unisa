---
aliases: 
tags:
  - corsi/informatica/sistemi_operativi
paragrafo: Sincronizzazione Processi
cssclasses:
  - 
---
Per risolvere la race condition si usa la:

![[Sezione Critica]]

## SOLUZIONE GENERALE
In generale qualsiasi soluzione al problema della sezione critica richiede l'uso di un semplice strumento, detto **Lock**.
Per accedere alla propria sezione critica, il processo deve *acquisire il possesso di un lock*, che poi *restituirà* al momento della sua uscita.

```C
while(1){
	acquisisce il lock
		/*sezione critica*/

	restituisce il lock
		/*sezione NON critica*/
}
```

---
### ALTERNANZA STRETTA
In questa soluzione semplice, si supponga che ci siano due processi $P_0$ e $P_1$ che utilizzano una variabile `turn` inizializzata a 0, utilizzata per gestire le sezioni d'entrata e uscita.

Il codice garantisce la stretta alternanza di ingresso alla sezione critica.
```C
// PROCESSO P0
while(1){
	while(turn != 0)
		;
		/*sezione critica*/
	turn = 1;
		/*sezione NON critica*/
}

// PROCESSO P1
while(1){
	while(turn != 1)
		;
		/*sezione critica*/
	turn = 0;
		/*sezione NON critica*/
}
```

Essenzialmente si verifica prima il valore di turn. Se è il turno dell'altro processo, quello in uscita imposta il `turn`.

> [!failure] Soluzione errata
> Questa soluzione *non è efficace*: perché se per qualsiasi motivo $P_0$ si attarda nella sua sezione non critica, $P_1$ rimane bloccato, violando il progresso (si ha [[Deadlock]]).

---
### SOLUZIONE DI PETERSEN
La **Soluzione di Petersen** è limitata a due processi, $P_0$ e $P_1$, ognuno dei quali esegue alternativamente la propria sezione critica e la sezione non critica.

I processi devono condividere i seguenti dati:
- *`int turn`*: segnala di chi sia il *turno* d'accesso alla sezione critica (quindi se `turn` è uguale a 0, sarà il turno di $P_0$);
- *`boolean flag[2]`*: indica se un processo *sia pronto* a entrare nella sezione critica (quindi se `flag[0]` è `true`, $P_0$ è pronto per entrare).

```C
while(1){
	flag[i] = true;
	j = 1-i; // Se i==1 allora j=0; Se i==0 allora j=1
	turn = j;
	
	while(flag[j] && turn == j) // Se è il turno di j e questo è pronto per entrare, aspetta
		;
		
		/*sezione critica*/
	flag[i] = false;
	
		/*sezione NON critica*/	
}
```

## HARDWARE DI SINCRONIZZAZIONE
Molti sistemi forniscono **supporto hardware** per evitare i problemi descritti precedentemente, per esempio *interdire le interruzioni* mentre si modificano le variabili condivise. 

Inoltre, molte architetture forniscono speciali [[Operazione atomica|istruzioni atomiche]] implementate in hardware: queste permettono di controllare e modificare il contenuto di una parola di memoria (*TestAndSet*), o di scambiare il contenuto di due parole di memoria (*Swap*).

### TEST AND SET
```C
bool TestAndSet(bool *object){
	bool value = *object;
	*object = true;
	return value;
}
```

Questa funzione viene eseguita senza interruzioni (*atomicamente*).
Gli viene passato l'indirizzo di una variabile booleana, il cui valore viene restituito alla fine. Prima di ritornare il valore, imposta a `true` la variabile originale.

```C
bool lock = false;

while(1){
	while(TestAndSet(&lock))
		;
	/*sezione critica*/
	lock = false;
	/*sezione NON critica*/
}
```

In questo codice si utilizza la funzione descritta prima.
Nel `while(TestAndSet(&lock))` si ha la prima volta `false`, mentre alla seconda `true`.
Alla fine si "restituisce" il `lock` impostandolo a `false`, in modo tale che un altro processo può entrare nella propria sezione critica.

Quindi `lock` è `true` quando *un processo è nella sua sezione critica*.

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