---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Thread
cssclasses:
  - 
---
## Lock Intrinseci
>Un **Lock Intrinseco** (o monitor lock) è una entità *associata ad ogni oggetto*. Garantisce sia l'*accesso esclusivo allo stato di un oggetto* che lo *stabilimento della relazione [[009 Errori con i Thread#^4112c2|happens-before]]*.

Per convenzione, un thread che ha bisogno di un accesso esclusivo e coerente ai campi di un oggetto *deve acquisire il lock intrinseco* dell'oggetto prima di accedervi e *rilasciare il lock* intrinseco quando ha finito. 

Si dice che un thread *possiede* il blocco intrinseco tra il momento in cui lo acquisisce e quello in cui lo rilascia. Finché un thread possiede un blocco intrinseco, *nessun altro* thread può acquisire lo stesso blocco, infatti gli altri thread si bloccano quando tentano di acquisire il blocco. Questo accade anche quando avviene una [[023 Gestione delle Eccezioni|eccezione]].

## Istruzioni Sincronizzate
A differenza dei [[011 Metodi Sincronizzati|metodi sincronizzati]], le **Istruzioni Sincronizzate** devono specificare l'oggetto che fornisce il lock intrinseco:

```Java
public void addName(String name) {
    synchronized(this) {
        lastName = name;
        nameCount++;
    }
    nameList.add(nome);
}
```

In questo modo, il metodo `addName` *deve sincronizzare le modifiche* a `lastName` e `nameCount`, ma deve anche evitare di sincronizzare le invocazioni dei metodi di altri oggetti.

Le istruzioni sincronizzate sono utili anche per migliorare la [[Concorrenza]] con una *sincronizzazione a grana fine*. 

> [!example]- <font color="orange">Esempio</font>
>Si supponga, per esempio, che la classe `MsLunch` abbia due campi di istanza, `c1` e `c2`, che non vengono mai usati insieme. Tutti gli aggiornamenti di questi campi devono essere sincronizzati, ma non c'è motivo di impedire che un aggiornamento di `c1` sia interlacciato con un aggiornamento di `c2`, e così facendo si riduce la concorrenza creando un blocco non necessario. Invece di usare metodi sincronizzati o di utilizzare in altro modo il blocco associato, creiamo due oggetti solo per fornire i blocchi.
>```Java
>public class MsLunch {
>    private long c1 = 0;
>    private long c2 = 0;
>    private Object lock1 = new Object();
>    private Object lock2 = new Object();
>
>    public void inc1() {
>	    // molto codice
>        synchronized(lock1) {
>            c1++;
>        }
>        // molto codice
>    }
>
>    public void inc2() {
>	    // molto codice
>        synchronized(lock2) {
>            c2++;
>        }
>        // molto codice
>    }
>}
>```


### Sincronizzazione di metodi statici
I metodi statici si riferiscono alla classe, quindi un *metodo statico e sincronizzato* previene l'esecuzione interfogliata da tutti gli altri metodi statici sincronizzati. In pratica, si acquisisce il lock dell'oggetto `ClassName.class`.

```Java
public class NomeClasse {
	public static void foo() {
		synchronized (NomeClasse.class) {
			// Body
		}
	}
}
```

> [!warning] Attenzione
> Metodi sincronizzati statici garantiscono accesso in mutua esclusione a metodi sincronizzati statici, mentre metodi sincronizzati di istanza garantiscono accesso in mutua esclusione a ai metodi sincronizzati di quella istanza.

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
- [Java Doc](https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html)