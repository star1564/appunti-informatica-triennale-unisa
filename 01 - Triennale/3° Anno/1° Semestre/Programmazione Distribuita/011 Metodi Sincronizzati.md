---
aliases: accesso concorrente
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Thread
cssclasses:
  - 
---
Java mette a disposizione vari strumenti efficaci per prevenire [[009 Errori con i Thread|degli errori più comuni]] durante l'esecuzione dei [[008 Thread in Java|thread]].

Uno di questi è la dichiarazione di **metodi sincronizzati**.

Per rendere un metodo sincronizzato, basta aggiungere la parola chiave **`synchronized`** alla sua dichiarazione:
```Java
public class SynchronizedCounter {
    private int c = 0;

    public synchronized void increment() {
        c++;
    }

    public synchronized void decrement() {
        c--;
    }

    public synchronized int value() {
        return c;
    }
}
```

Se `count` è un'istanza di `SynchronizedCounter`, la sincronizzazione di questi metodi ha due effetti:
- In primo luogo, *non è possibile che due invocazioni* di metodi sincronizzati sullo stesso oggetto *si sovrappongano*. Quando un thread esegue un metodo sincronizzato per un oggetto, tutti gli altri thread che invocano metodi sincronizzati per lo stesso oggetto *si bloccano* (sospendono l'esecuzione) finché il primo thread non ha finito con l'oggetto;
‎ %%← Empty Char%%
- In secondo luogo, quando un metodo sincronizzato esce, stabilisce automaticamente una relazione [[009 Errori con i Thread#^4112c2|happens-before]] con *ogni successiva invocazione* di un metodo sincronizzato per lo stesso oggetto. Questo garantisce che le modifiche allo stato dell'oggetto siano visibili a tutti i thread.

Da notare che i costruttori non possono essere sincronizzati: usare la parola chiave `synchronized` con un costruttore è un errore di sintassi. La sincronizzazione dei costruttori non ha senso, perché solo il thread che crea l'oggetto dovrebbe avere accesso all'oggetto durante la sua costruzione.

> [!warning] Attenzione
>Quando si costruisce un oggetto che sarà condiviso tra i thread, bisogna fare molta attenzione che un riferimento all'oggetto non "perda" prematuramente. Per esempio, si supponga di voler mantenere una [[Lista (Java)|lista]] chiamata `instances` contenente ogni istanza della classe. Si potrebbe essere tentati di aggiungere la seguente riga al proprio costruttore:
>```Java
>instances.add(this);
>```
>Ma poi altri thread possono usare `instances` per accedere all'oggetto prima che la costruzione dell'oggetto sia completata.

---
> [!example]- <font color="orange">Esempio</font>
>```Java
>public class SharedResourceExample {  
>    private int sharedVar = 0;  
>  
>    public synchronized void incrementVar(int amount) {  
>       // Sezione critica:  
>       // solo un thread alla volta può eseguire questo metodo      
>       sharedVar += amount;  
>    }  
>  
>    public int getSharedVar() {  
>       return sharedVar;  
>    }  
>  
>    public static void main(String[] args) {  
>       SharedResourceExample sharedResource = new SharedResourceExample();  
>  
>  
>       Thread thread1 = new Thread(() -> {  
>          for (int i = 0; i < 1000; i++) {  
>             sharedResource.incrementVar(2);     // 2000  
>          }  
>       });  
>  
>       Thread thread2 = new Thread(() -> {  
>          for (int i = 0; i < 1000; i++) {  
>             sharedResource.incrementVar(-1);    // -1000  
>          }  
>       });  
>         
>       thread1.start();  
>       thread2.start();  
>  
>       // Attendere che i thread terminino  
>       try {  
>          thread1.join();  
>          thread2.join();  
>       } catch (InterruptedException e) {  
>          e.printStackTrace();  
>       }  
>  
>       // Output: 1000  
>       System.out.println("Valore finale della risorsa condivisa: " + sharedResource.getSharedVar());  
>         
>    }  
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
- [Java Doc](https://docs.oracle.com/javase/tutorial/essential/concurrency/syncmeth.html)