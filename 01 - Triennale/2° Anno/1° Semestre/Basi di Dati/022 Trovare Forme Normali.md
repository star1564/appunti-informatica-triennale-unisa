---
aliases: 
tags:
  - corsi/informatica/basi_di_dati
paragrafo: Forme Normali
cssclasses:
  - 
---
> [!note]- Traccia
> ![[Pasted image 20230110163354.png]]



---
### 1) TROVARE LE CHIAVI DI UNO SCHEMA
È molto raro che negli esercizi vengano date direttamente le chiavi di uno schema, quindi bisogna trovarle.

Si tracciano le [[020 Dipendenze Funzionali|dipendenze funzionali]] indicate:
![[Pasted image 20230110164134.png|650]]

Ora si cercano le chiusure degli attributi da cui partono le dipendenze:
- $\color{#CC241D}\{C\}^+ = \{C, D, A, H, G\}$, algoritmo:
	- Si vede che da $C$ partono delle dipendenze, pertanto si aggiunge $C$ alla chiusura $\{C\}^+$;
	- $C➔D$, aggiungi $D$ e vedi la sua chiusura $\{D\}^+$:
		- $D➔A$, aggiungi $A$ e vedi la sua chiusura $\{A\}^+$, questa è vuota quindi si continua.
		- $D➔H$, aggiungi $H$ e vedi la sua chiusura $\{H\}^+$, questa è vuota quindi si continua.
	- $C➔G$, aggiungi $G$ e vedi la sua chiusura $\{G\}^+$, questa è vuota quindi si continua.
- $\color{#CC241D}\{E\}^+ = \{E, F, B, A\}$, algoritmo:
	- Si vede che da $E$ partono delle dipendenze, pertanto si aggiunge $E$ alla chiusura $\{E\}^+$;
	- $E➔F$, aggiungi $F$ e vedi la sua chiusura $\{F\}^+$, questa è vuota quindi si continua;
	- $E➔B$, aggiungi $B$ e vedi la sua chiusura $\{B\}^+$:
		- $B➔A$, aggiungi $A$ e vedi la sua chiusura $\{A\}^+$, questa è vuota quindi si continua.
 - $\color{#CC241D}\{C,E\}^+ = \{C, D, A, H, G, E, F, B\} = R$, algoritmo:
	- Si vede che da $C,E$ parte una delle dipendenze, pertanto si aggiunge $C,E$ alla chiusura $\{C,E\}^+$;
	- Si nota e inserisce la chiusura $\{C\}^+$;
	- Si nota e inserisce la chiusura $\{E\}^+$.
 
Quindi, visto che $\{C,E\}^+ = R$, $CE$ è minimale. Pertanto è anche la chiave. Vengono segnate nel disegno.

![[Pasted image 20230110165447.png|650]]

---
### 2) FORMA NORMALE 2
Ora si cercano le [[021 Forme Normali|forme normali]]. 
Di solito negli esercizi, si ha già la forma normale 1, quindi si cerca la 2.

Notiamo che le seguenti dipendenze funzionali violano la [[021 Forme Normali#SECONDA FORMA NORMALE (2NF)|2NF]]:
- $C➔\{D,G\}$
- $E➔\{B,F\}$

Si prendono le chiusure di $C$ ed $E$ come base per le nuove relazioni.
![[Pasted image 20230110165908.png|400]]

La chiave deve essere presente almeno un una relazione. In questo caso abbiamo come chiave $\{C,E\}$, ma non esistono entrambe nelle prime due relazioni, pertanto se ne crea una nuova ($R_3$).

---
### 3) FORMA NORMALE 3
Notiamo che le seguenti dipendenze funzionali violano la [[021 Forme Normali#TERZA FORMA NORMALE (3NF)|3NF]]:
- $D➔\{A,H\}$
- $B➔A$

Si separano le relazioni $R_1$ e $R_2$ in questo modo:

![[Pasted image 20230110170229.png]]

___
[[000 Indice BD|↖ Ritorna all'indice ↖]]

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