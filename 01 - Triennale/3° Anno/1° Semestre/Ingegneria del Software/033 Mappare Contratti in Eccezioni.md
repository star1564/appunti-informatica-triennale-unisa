---
aliases: 
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Implementazione
cssclasses:
  - 
---
La violazione di un [[Contratti|Contratto]] genera un errore del sistema che deve essere gestito. Questo può essere fatto attraverso le [[023 Gestione delle Eccezioni|Eccezioni]] nel codice.

Ogni violazione di un qualsiasi tipo di contratto *corrisponde ad una specifica eccezione*. Si possono usare le eccezioni di default del linguaggio o si possono [[023 Gestione delle Eccezioni#ECCEZIONI PERSONALIZZATE|crearne delle nuove]]. 

Si verifica attraverso un `if`, dove, se il contratto risulta violato, verrà lanciata una eccezione contenente delle informazioni pertinenti all'errore.

Un semplice mapping prevede il controllo dei tre tipi di contratti:
- **Controllare le [[Contratti#^e78e08|Precondizioni]]**: devono essere *controllate all'inizio del metodo*, prima che si eseguano dei processi. 
- **Controllare le [[Contratti#^438049|Postcondizioni]]**: devono essere *controllate alla fine del metodo*, dopo che tutto il lavoro è stato completato.
- **Controllare le [[Contratti#^fba292|Invarianti]]**: devono essere *controllate insieme alle postcondizioni*.

In caso si abbia [[015 Ereditarietà|Ereditarietà]], il codice per controllare se i contratti sono stati violati deve essere incapsulato in metodi separati che possono essere chiamati dalle sottoclassi.

> [!tip] Euristiche per il mapping dei contratti
>- *È possibile omettere il codice di controllo per le postcondizioni e gli invarianti*: è ridondante inserirlo insieme al codice che realizza la funzionalità della classe, inoltre, non individua molti bug.
>- *È possibile omettere il codice di controllo per metodi privati e protetti* se è ben definita l'interfaccia del sottosistema.
>- *Concentrarsi sui contratti delle componenti con una lunga vita*: come gli [[014 Modello ad Oggetti#^7cb8de|oggetti Entity]].
>- *Riusare il codice per il controllo dei contratti*: molte operazioni hanno precondizioni simili e si incapsula il codice per il controllo degli stessi vincoli in metodi così possono condividere le stesse classi di eccezioni.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240125123333.png|800]]
>
>```Java
>public class Tournament {
>	//...
>	private List players;
>	
>	public void addPlayer(Player p) throws KnownPlayer, TooManyPlayers, UnknownPlayer,
>		IllegalNumPlayers, IllegalMaxNumPlayers {
>		// --------- PRECONDITIONS ---------
>		// check precondition !isPlayerAccepted(p)
>		if (isPlayerAccepted(p)) {
>			throw new KnownPlayer(p);
>		}
>	
>		// check precondition getNumPlayers() < maxNumPlayers
>		if (getNumPlayers() == getMaxNumPlayers()) {
>			throw new TooManyPlayers(getNumPlayers());
>		}
>		
>		// save values for postconditions
>		int pre_getNumPlayers = getNumPlayers();
>	
>		// --------- REAL WORK ---------
>		players.add(p);
>		p.addTournament(this);
>		
>		// --------- POSTCONDITIONS ---------
>		// check post condition isPlayerAccepted(p)
>		if (!isPlayerAccepted(p)) {
>			throw new UnknownPlayer(p);
>		}
>		
>		// check post condition getNumPlayers() = @pre.getNumPlayers() + 1
>		if (getNumPlayers() != pre_getNumPlayers + 1) {
>			throw new IllegalNumPlayers(getNumPlayers());
>		}
>		
>		// check invariant maxNumPlayers > 0
>		if (getMaxNumPlayers() <= 0) {
>			throw new IllegalMaxNumPlayers(getMaxNumPlayers());
>		}
>	}
>	//...
>}
>```


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