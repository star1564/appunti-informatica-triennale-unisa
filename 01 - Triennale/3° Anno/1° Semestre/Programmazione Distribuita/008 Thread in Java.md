---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Thread
cssclasses:
  - 
---
>I [[023 Thread|Thread]] in Java sono degli oggetti, quindi istanze della classe `Thread`. 

## Creazione di un Thread
In Java si possono creare in due modi.

### Ereditarietà
Si estende la classe personalizzata attraverso l'[[015 Ereditarietà|Ereditarietà]] con la classe `Thread`.

```Java
class MioThread extends Thread {
    public void run() {
        // Codice da eseguire nel thread
        System.out.println("Il mio thread sta eseguendo.");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creare un'istanza del thread personalizzato
        MioThread mioThread = new MioThread();
        
        // Avviare il thread
        mioThread.start();
    }
}
```

### Interfaccia
Si fa implementare una [[018 Interfacce|Interfaccia]] `Runnable` alla classe personalizzata. Come per le interfacce normali, questa soluzione *offre un utilizzo generale*.

```Java
class MioRunnable implements Runnable {
    public void run() {
        // Codice da eseguire nel thread
        System.out.println("Il mio thread sta eseguendo.");
    }
}

public class Main {
    public static void main(String[] args) {
        // Creare un thread utilizzando l'istanza Runnable
        Thread thread = new Thread(new MioRunnable());
        
        // Avviare il thread
        thread.start();
    }
}
```

### Lambda
Si utilizza una [[021 Espressioni Lambda|espressione lambda]] per semplificare la creazione di un thread.

```Java
public class Main {
    public static void main(String[] args) {
		
        Thread thread = new Thread(() -> {
	        // Lavoro che il thread deve eseguire
	        for(int i = 0; i < 10; i++){
		        System.out.println("i: " + i);
	        }
        });
        
        // Avviare il thread
        thread.start();
    }
}
```


## Alcuni metodi utili

```Java
public class EsempioThread {
    public static void main(String[] args) {
        // Creazione di un oggetto Runnable che rappresenta il lavoro del thread
        Runnable lavoro = new Runnable() {
            public void run() {
                for (int i = 1; i <= 5; i++) {
                    System.out.println("Thread in esecuzione: " + i);
                    
                    try {
                        Thread.sleep(1000); // Mette il thread in pausa per 1 secondo
                        // 1000 ms = 1 sec
                    } catch (InterruptedException e) {
	                    // Se il thread viene interrotto genera una eccezione
                        e.printStackTrace();
                    }
                }
            }
        };

        // Creazione di due thread che eseguono lo stesso lavoro
        Thread thread1 = new Thread(lavoro);
        Thread thread2 = new Thread(lavoro);

        // Avvio dei thread
        thread1.start();
        thread2.start();

        // Attendere che entrambi i thread abbiano completato il loro lavoro
        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Entrambi i thread hanno completato il lavoro.");
    }
}

```

1. Viene creato un oggetto `Runnable` che *definisce il lavoro da svolgere* nel thread (nella funzione **`run()`**), che consiste nella stampa di una frase e di un *attesa di un determinato tempo in millisecondi* attraverso la **`Thread.sleep()`**;
2. Vengono creati due thread (`thread1` e `thread2`) che eseguiranno *lo stesso lavoro*;
3. I thread vengono *avviati* utilizzando il metodo **`start()`**;
4. Dopo aver avviato i thread, il programma principale *attende che entrambi abbiano completato il loro lavoro* utilizzando **`join()`**;
5. Alla fine, il programma stampa un messaggio per indicare che entrambi i thread hanno completato il lavoro.

---
> [!info]- [[UML 3 - Diagramma di Stato|Diagramma di Stato]] di un Thread
> ![[Pasted image 20230929184358.png|750]]

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