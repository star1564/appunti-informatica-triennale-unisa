---
aliases:
  - ODD
  - Modello di Object Design
tags:
  - corsi/informatica/ingegneria_software
paragrafo: Progettazione - Object Design
cssclasses:
  - 
---
>L'**Object Design Document (ODD)** contiene il risultato ottenuto [[023 Object Design|Object Design]]. Descrive i trade-off affrontati dagli sviluppatori, la decomposizione completa del sistema e linee di guida per le interfacce dei sottosistemi e delle classi. Serve per *scambiare informazioni sulle interfacce* tra i diversi team e come riferimento per il [[035 CVS - Testing|Testing]].

In questa fase si vuole ottenere un **Modello di Object Design**.

Ci sono tre tipi di approcci per creare questo documento:
- **Documentazione in maniera AUTONOMA dell'ODD** (usare questo per il progetto): consiste nel creare l'ODD allo stesso modo in cui si è creato il [[017 Requirement Analysis Document|RAD]] oppure l'[[022 System Design Document|SDD]]. Si scrive e si mantiene un modello UML e si genera il documento. Questo documento duplicherebbe tutti gli [[023 Object Design#Oggetti di Applicazione e Soluzione|Oggetti di Applicazione]] identificati durante l'analisi. 
	- Gli svantaggi di questa soluzione includono la *ridondanza con il RAD* e un elevato livello di *sforzo per mantenere la coerenza con il RAD*. Questo porta spesso a un RAD e a un ODD imprecisi o non aggiornati.
- *ODD come ESTENSIONE del RAD*: l'object design è considerato come una serie di oggetti di applicazione augmentati con [[023 Object Design#Oggetti di Applicazione e Soluzione|Oggetti di Soluzione]].
	- Il vantaggio di questa soluzione è che il mantenimento della coerenza tra il RAD e l'ODD diventa molto più semplice grazie alla riduzione della ridondanza. 
	- Gli svantaggi di questa soluzione includono l'*inquinamento del RAD con informazioni irrilevanti per il cliente e l'utente*.
- *ODD INCORPORATO nel codice sorgente*: come nel primo approccio, si rappresenta l'ODD usando uno strumento di modellazione e, una volta che l'ODD diventa stabile, si descrivono ogni interfaccia di classe usando *commenti etichettati* che distinguono i commenti del codice sorgente dalle descrizioni della progettazione degli oggetti. Si può fare ciò attraverso dei *tool specializzati* (come [[Annotazioni#ANNOTAZIONI DESCRITTIVE (JAVADOC)|Javadoc]]). 
	- Il vantaggio di questo approccio è che la coerenza tra il modello di progettazione degli oggetti e il codice sorgente *è molto più facile da mantenere*: quando vengono apportate modifiche al codice sorgente, i commenti taggati vengono aggiornati e l'ODD rigenerato.

> [!tip] <font color="orange">Template</font>
>```
>1. Introduzione
>	1.1 Trade-off dell'Object Design
>	1.2 Linee guida per le Interfacce
>	1.3 Definizioni, acronimi ed abbreviazioni
>	1.4 Riferimenti
>2. Diagramma di Classi
>3. OCL
>4. Glossario
>5. Packages
>6. Interfacce di Classe
>```

> [!warning] Non va inserito codice o le tecnologie utilizzate
> Va inserito anche l'[[026 Object Constraint Language|OCL]] nel documento.


### Javadoc con OCL
> [!warning] Non si dovrebbe utilizzare l'OCL in Javadoc per il progetto
> Lo inserisco qui solo per dimostrazione.

È possibile usare le annotazioni per ogni tipo di [[Contratti|Contratto]].

| Annotazione | Contratto |
| ---- | ---- |
| `@invariant` | Invariante |
| `@pre` | Precondizione |
| `@post` | Postcondizione |

> [!example]- <font color="orange">Esempio di Javadoc</font>
>```Java
>/** A Tournament is a series of Matches among a set of Players
>* which ends with a single winner. The Game and TournamentStyle of a
>* TournamentStyle is determined by the League in which the Tournament is
>* played.
>*
>* The Tournament starts empty, a number of Players are accepted in
>* the Tournament, the Tournament is planned, and finally,
>* the Matches are played.
>*
>* Invariants:
>* The maximum number of players is positive at all times.
>* @invariant getMaxNumPlayers > 0
>* The number of players is always less or equal than the max number.
>* @invariant getPlayers().size() < getMaxNumPlayers()
>* The initial attributes of the Tournament are not null.
>* @invariant getLeague() != null and getName() != null
>*/
>
>public class Tournament {
>	/* Fields omitted */
>	
>	/** Public constructor. A Tournament starts with a league, a name,
>	* and a maximum number of players. These attributes cannot be changed.
>	*/
>	public Tournament(League league, String name, int maxNumPlayers) {…}
>	
>	/** Returns a list of the current Players
>	*/
>	public List getPlayers() {…}
>	
>	/** This operation accepts a new Player in the Tournament.
>	* @pre !isPlayerAccepted(p)
>	* @pre getPlayers().size() < getMaxNumPlayers()
>	* @post isPlayerAccepted(p)
>	* @post getPlayers().size() = self@pre.getPlayers().size() + 1
>	*/
>	public void acceptPlayer (Player p) {…}
>	
>	/** The removePlayer() operation assumes that the specified player
>	* is currently in the Tournament.
>	* @pre isPlayerAccepted(p)
>	* @post !isPlayerAccepted(p)
>	* @post getPlayers().size() = self@pre.getPlayers().size() - 1
>	*/
>	public void removePlayer(Player p) {…}
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