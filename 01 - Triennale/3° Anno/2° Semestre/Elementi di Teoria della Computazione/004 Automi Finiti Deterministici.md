---
aliases:
  - DFA
  - Diagramma di stato (Automi)
  - Funzione di Transizione
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Deterministici
cssclasses:
  - 
---
### Automi Finiti
>Gli **Automi Finiti** (o macchine a stato) sono meccanismi di computazione *"primitivi"* con poca capacità di memoria. Il loro **stato** può essere specificato attraverso:
>- *Tabelle di transizioni di stato*
>- *Diagrammi di stato*

^6217d1

È essenzialmente una macchina che **accetta o rifiuta** un input.

Vengono usati in molte situazioni, da semplici dispositivi elettronici, riconoscitori per il pattern matching, fino agli analizzatori lessicali dei compilatori.

> [!example]- <font color="orange">Esempio</font>
>Il sistema di controllo per una porta automatica è un esempio di tale dispositivo. Le porte automatiche ad anta battente, che si trovano spesso alle entrate e uscite dei supermercati, si aprono quando il sistema di controllo avverte che una persona si sta avvicinando. Ha dei sensori anteriori e posteriori per rilevare la presenza delle persone su entrambi i lati.
>
>![[Pasted image 20240310143629.png|300]]
>
>Il sistema di controllo è in uno dei due stati: ***`OPEN`*** o ***`CLOSED`***, che rappresentano la condizione corrispondente della porta. 
>
>Ci sono quattro possibili condizioni di input: 
>- **`FRONT`** - quando una persona è sul sensore anteriore
>- **`REAR`** - quando una persona è sul sensore posteriore 
>- **`BOTH`** - quando vi sono persone su entrambi i sensori
>- **`NEITHER`** - quando nessuno è su alcun sensore
>
>Queste quattro condizioni si dicono *stati* e possono essere rappresentati in due modi.
>
>- *Tabella delle transizioni*: ![[Pasted image 20240310144253.png|600]]
>
>- *Diagramma di stati*: ![[Pasted image 20240310144206.png|400]]
>Il sistema di controllo *passa da uno stato a un altro*, in base all'input che riceve:
>- Nello stato ***`CLOSED`***, se riceve l'input **`NEITHER`**, **`BOTH`** oppure **`REAR`**, resta nello stato ***`CLOSED`***. 
>	- Se riceve l'input **`FRONT`**, passa nello stato ***`OPEN`***. 
>
>- Nello stato ***`OPEN`***, se riceve l'input **`FRONT`**, **`REAR`** oppure **`BOTH`** resta in ***`OPEN`***. 
>	- Se riceve l'input **`NEITHER`**, ritorna a ***`CLOSED`***. 

### Deterministici
>Questi tipi di automi vengono chiamati **Deterministici** (quindi *DFA*). 

Il termine "deterministico" si riferisce al fatto che *per ogni input avviene ESATTAMENTE una e una sola transizione* da uno stato all'altro *per ogni elemento dell'alfabeto*. ^determinismo

Quindi, quando la macchina è in un dato stato e legge il simbolo di input successivo, sappiamo qual è lo stato successivo.


## Diagrammi di Stato per gli Automi
>Un **Diagramma di Stato** (simile a quello dell'[[UML 3 - Diagramma di Stato|UML]]) serve per rappresentare tutti gli *stati* nel quale si può trovare l'automa.

![[Pasted image 20240310150941.png|600]]

Un diagramma è composto da:
- **Stati** - rappresentano una *situazione* in cui l'automa si può ritrovare, infatti conserva informazioni importanti. Sono indicati con dei cerchi $(q_0,q_1,q_2,q_3,q_4)$.
- **Transizioni** - quando accade un evento, l'automa *cambia lo stato attuale* ad un altro stato attraverso una transazione. Sono indicati con degli archi con frecce che vanno da uno stato all'altro.
	- Una transazione può anche *partire ed arrivare sullo stesso stato*. Questo viene chiamato **cappio** (*ciclo* o *loop*).

Esistono tre tipi di stati:
- Il diagramma *deve avere esattamente uno* stato chiamato **stato iniziale**, indicato dall'arco entrante in esso e non uscente da uno stato $(q_0)$. 
	- È il punto in cui inizia *l'elaborazione dell'automa*.

^statoIniziale

- Il diagramma *può avere da zero a più* stati chiamati **stati accettanti**, indicati con un doppio cerchio $(q_3)$. 
	- È il punto in cui un input viene *accettato*. 

^statoAccettante

- Tutti gli stati che *non sono accettanti*, vengono chiamati **stati non accettanti** $(q_0,q_1,q_2,q_4)$. 
	- Se l'automa rimane bloccato in uno di essi, l'input viene *rifiutato* fino a quando non riesce ad uscirne e ad arrivare ad uno stato accettante.
	- Se uno stato non accettante *non ha archi uscenti* $(q_4)$, viene detto **stato pozzo** (o trappola). Infatti non è possibile uscirne, pertanto l'input viene *rifiutato* direttamente. ^statoPozzo

^statoNonAccettante

> [!tip] L'automa accetta anche la stringa vuota se e solo se lo stato iniziale è anche lo stato accettante

---
> [!example]- <font color="orange">Esempio Accettazione (PDF)</font>
>![[DFA esempio 1.pdf]]

> [!example]- <font color="orange">Esempio Rifiuto (PDF)</font>
>![[DFA esempio 2.pdf]]

> [!example]- <font color="orange">Esempio Stringa Vuota (PDF)</font>
>![[DFA esempio 3.pdf]]

## Definizione Formale
>Un **Automa Finito Deterministico** è una quintupla $\color{#61AFEF}(Q,\Sigma,\delta,q_0,F)$, dove:
>1. $\color{#61AFEF}Q$ è un insieme finito chiamato l'**insieme degli stati**
>2. $\color{#61AFEF}\Sigma$ è un insieme finito chiamato l'**alfabeto**
>3. $\color{#61AFEF}\delta:Q\times\Sigma\to Q$ è la **funzione di transizione**, che definisce le regole per il cambiamento di stato
>4. $\color{#61AFEF}q_0\in Q$ è lo **stato iniziale**
>5. $\color{#61AFEF}F\subseteq Q$ è l'**insieme degli stati accettanti**

### Funzione di transizione
La [[011 Funzioni|funzione]] di transizione definisce le regole per il cambiamento di stato e può essere rappresentata in *forma esplicita* oppure attraverso una *tabella di transizione*.

> [!example]- <font color="orange">Esempio Funzione di transizione</font>
>![[Pasted image 20240310162528.png|400]]
>
>- $Q=\{x,y,z\}$
>- $\Sigma=\{0,1\}$
>- $\delta$
>- $q_0=x$
>- $F=\{y\}$
>
>Possiamo indicare le transizioni di stato attraverso le funzioni di transizione: 
>$$\delta(x,0)=z$$
>$$\delta(x,1)=y$$
>$$\delta(y,0)=z$$
>$$\delta(y,1)=y$$
>$$\delta(z,0)=z$$
>$$\delta(z,1)=z$$
>
>Si può anche specificare la tabella degli stati:
>![[Pasted image 20240310162721.png|250]]
>Dove $x$ è lo stato iniziale (che si indica con una freccia) e $y$ uno stato accettante (che si indica con un asterisco).


#
___
[[000 Indice ETC|↖ Ritorna all'indice ↖]]

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
- [lydia - Video](https://www.youtube.com/watch?v=PK3wL7DXuuw&list=PLhqug0UEsC-IDomfNsn8e3neoy34o8oye&index=3)