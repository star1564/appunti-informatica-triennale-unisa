---
aliases: 
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java RMI
cssclasses:
  - 
---
> **Java RMI** (Remote Method Invocation) è una libreria che *permette l'invocazione di metodi* su [[016 Oggetti Remoti|Oggetti Remoti]].

Le applicazioni RMI seguono tipicamente una architettura [[Modello Server-Client|Client-Server]], dove il server crea un certo numero di oggetti `server` accessibili da remoto e attende che gli oggetti `client` ne utilizzino i servizi.

### Gli obbiettivi principali di Java RMI
- *Invocazione* semplice e trasparente di metodi remoti;
- *Integrazione* completa e naturale con Java;
- Avere un *[[014 Garbage Collector|Garbage Collector]] Distribuito* per evitare i [[Memory Leak]] attraverso il reference counting (ovvero una memoria condivisa tra tutti i nodi). Quando viene rimosso un oggetto, lo comunica al server per far cancellare l'oggetto anche li;
- *Individuare* facilmente in fase di progettazione ed implementazione se un oggetto *possa essere locale o remoto*, infatti il programmatore deve avere la consapevolezza che sta usando un oggetto remoto;
- Preservare il modello di *sicurezza* fornito da Java.

### Sicurezza in Java
I programmi o applet vengono eseguiti all'interno di una *sandbox* nella quale le sue operazioni non risultano pericolose.

![[Pasted image 20231013163242.png|500]]

Fornisce la sandbox basandosi su quattro livelli di sicurezza:
1. *Sicurezza del Linguaggio*: 
	- Java è fortemente tipizzato, ovvero tutte le variabili hanno un tipo definito a tempo di compilazione. 
	- Offre la gestione automatica della memoria con il garbage collection che impedisce di esaurire lo spazio di indirizzamento del processo.
	- Non ha puntatori per rendere impossibile l'accesso illegale alla memoria.

2. *Classloader*: si occupa di caricare la classe a tempo di esecuzione, anche da locazioni remote. Le carica in un namespace separato rispetto a quello delle classi locali, questo in modo che classi del linguaggio built-in e quelle locali non vengano sovrascritte da altre.

3. *Bytecode Verifier*: controlla che una classe caricata sia conforme alle specifiche del linguaggio e che non ci siano violazioni alle regole.

4. *Security Manager*: si occupa della sandbox. Viene interpellato dalla JVM per ciascuna operazione potenzialmente pericolosa e fornisce le autorizzazioni sulla base della politica che ha stabilito l'utente lanciando la macchina virtuale.

### Unicast e Multicast
Java RMI prevede la possibilità che esistano diversi tipi di invocazione:
- **Unicast**: un tipo di trasferimento di informazioni che viene utilizzato quando vi è la partecipazione di *un singolo mittente* e di *un singolo destinatario*;
- **Multicast**: dove *uno o più mittenti* e *più destinatari* partecipino al traffico di trasferimento dati. 

![[Pasted image 20231013155808.png|400]]

### Passaggio di Parametri
Un metodo remoto può dichiarare solo parametri o valori restituiti che siano [[025 Serializzazione|serializzabili]]. Inoltre, un oggetto locale passato come parametro o restituito come valore, viene passato **per copia**. Ovvero, il contenuto dell'oggetto *viene copiato prima di essere serializzato*. 

Questo comportamento è diverso della meccanica principale di Java dove viene passato ad un metodo il riferimento di un oggetto.

### Marshalling e Serializzazione
Essenzialmente la serializzazione trasforma un oggetto in uno stream di byte.

Invece il **Marshalling** utilizza il flusso di serializzazione modificando varie cose, tra cui:
- la semantica degli oggetti remoti;
- la scrittura di informazioni aggiuntive sull'oggetto.




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