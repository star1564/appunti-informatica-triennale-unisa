---
aliases:
  - NFA
  - automa finito non deterministico
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Non Deterministici
cssclasses:
  - 
---
>Un **[[004 Automi Finiti Deterministici#^6217d1|Automa a stati finiti]] Non Deterministico** (*NFA*, Nondeterministic Finite Automaton) può trovarsi *contemporaneamente in diversi stati*. Quindi, per ogni stato successivo di ogni punto, possono esistere diverse scelte.

> [!tip] Il non determinismo è una generalizzazione del determinismo, quindi ogni automa finito deterministico [[012 Linguaggio di un NFA#Equivalenza NFA-DFA|è anche un automa finito non deterministico]]

^0ca79f

![[Pasted image 20240320122243.png|600]]

In un NFA:
- Uno stato può avere *zero, uno o più* archi uscenti *per ogni simbolo* dell'alfabeto (ad esempio, $q_1$ ha un arco uscente per 0, ma ne ha due per 1, invece $q_3$ non ha un arco uscente per 0)
- Si possono avere archi etichettati con *elementi dell'alfabeto o la stringa vuota $\epsilon$*, quest'ultima viene chiamata **$\epsilon$-transizione** (da tenere in mente che in questo modo NON viene aggiunta $\epsilon$ all'alfabeto)

> [!summary] Come computa un NFA
>Supponiamo di eseguire un NFA su una stringa di input e di giungere in uno stato con più modi di procedere (come nello stato $q_1$ della figura di sopra quando si ha un input 1).
>
>- Dopo aver letto questo simbolo, la macchina *si divide in più copie* di sé stessa e segue *tutte* le possibilità **in parallelo**. Ogni copia della macchina prende una delle possibili direzioni per procedere e continua come prima. 
>	- Se ci sono scelte successive, la macchina si divide di nuovo.
>	- Accade una cosa simile se ci imbattiamo in uno stato con un simbolo $\color{#CC241D}\epsilon$ su un arco uscente. *Senza leggere alcun input*, la macchina si divide in più copie multiple, una che segue ciascun arco uscente etichettato con $\epsilon$ e una che resta nello stato corrente.
>- Se il simbolo di input successivo *non compare su alcuno degli archi uscenti* dallo stato occupato da una copia della macchina, quella copia della macchina **"muore" insieme al ramo della computazione** a essa associato.
>- Infine, se *una qualunque* di queste copie della macchina è in uno stato accettante *alla fine dell'input*, l'NFA **accetta** la stringa in input.

Un modo per considerare una computazione non deterministica è vederla come un *albero di possibilità* ([[028 Alberi di Decisione|Decision Tree]]). La radice dell'albero corrisponde all'inizio della computazione. Ogni punto di ramificazione nell'albero corrisponde a un punto nella computazione in cui la macchina ha scelte multiple. 

![[Pasted image 20240320124502.png]]

Se da un possibile stato all'interno di un livello esce un arco etichettato con $\epsilon$, si deve anche considerare sullo stesso livello lo stato che $\epsilon$ va a toccare.


> [!example]- <font color="orange">Esempi (PDF)</font>
>![[Esempio NFA.pdf]]

> [!example]- <font color="orange">Esempio computazione come albero</font>
>Consideriamo l'esecuzione del seguente NFA con l'input 010110.
>
>![[Pasted image 20240320122243.png|600]]
>
>![[Pasted image 20240320140832.png]]
>
>Si comincia da $q_1$.
>- 0 - Da $q_1$ c'è solo un posto dove andare su uno 0, ossia su $q_1$, quindi restiamo là.
>- 1 - In $q_1$ su un 1 ci sono due scelte: restare in $q_1$ o muoversi in $q_2$. La macchina si divide in due per seguire ciascuna scelta, non deterministicamente. Una $\epsilon$-transizione esce dallo stato $q_2$, quindi la macchina si divide di nuovo. Questo perché la $\epsilon$-transizione *consente di arrivare anche in $q_3$ se posso arrivare in $q_2$*. A questo livello abbiamo $q_1,q_2,q_3$.
>- 0 - Il processo che stava su $q_3$ muore, perché non ha archi su 0 da seguire, mentre gli altri continuano.
>- ...
>
>Si può notare che questo automa accetta tutte le stringhe che contengono 101 oppure 11 come sottostringa. DFA corrispondente:
>
>![[Pasted image 20240320142150.png|600]]

## Esprimere linguaggi con NFA
Esprimere linguaggi con gli NFA risulta essere molto più semplice e conciso rispetto ai DFA.
### Linguaggi Vuoti per NFA
Il [[005 Linguaggio di un DFA|Linguaggio Riconosciuto]] da un NFA è vuoto se lo stato iniziale non è accettante e non ha archi uscenti.

![[Pasted image 20240320142637.png|180]]

### Linguaggi con Stringa Vuota per NFA
Il Linguaggio Riconosciuto da un NFA è composto solamente dalla stringa vuota se lo stato iniziale è accettante e non ha archi uscenti.

![[Pasted image 20240320142742.png|180]]

Infatti, la stringa vuota è accettata se: 
- *lo stato iniziale è accettante*
- esiste un path dallo stato iniziale ad uno stato finale ottenuto seguendo *esclusivamente transizioni etichettate con la stringa vuota*.

> [!example]- <font color="orange">Esempio</font>
>Progettare un automa finito che riconosce stringhe su $\{a,b,c\}$ della forma $a^ib^jc^k$, dove $i,j,k$ sono maggiori o uguali a zero.
>
>![[Pasted image 20240320143439.png|700]]
>
>---
>![[Pasted image 20240320164139.png|400]]
>
>Questo diagramma (con $\Sigma=\{0\}$) accetta tutte le stringhe della forma $0^k$ con $k$ multiplo di 2 o di 3.
>
>È quindi una sorta di unione, dove i multipli di 2 vengono controllati sopra, quelli di 3 sotto.


## Definizione formale di NFA
La definizione formale di automa finito non deterministico è simile a quella di un [[004 Automi Finiti Deterministici#Definizione Formale|automa finito deterministico]]. Entrambi hanno stati, un alfabeto di simboli di input, una [[004 Automi Finiti Deterministici#Funzione di transizione|Funzione di Transizione]], uno stato iniziale, e una collezione di stati accettanti. 

Comunque, essi differiscono in un aspetto essenziale: nel *tipo* di funzione di transizione. In un NFA, la funzione di transizione prende uno *stato* e un *simbolo* di input *o la stringa vuota* e produce l'**insieme dei possibili stati successivi**. 

>Un **Automa Finito Non Deterministico** è una quintupla $\color{#61AFEF}(Q,\Sigma,\delta,q_0,F)$, dove:
>1. $\color{#61AFEF}Q$ è un insieme finito chiamato l'**insieme degli stati**
>2. $\color{#61AFEF}\Sigma$ è un insieme finito chiamato l'**alfabeto**
>3. $\color{#61AFEF}\delta:Q\times\Sigma_\epsilon\to \mathcal{P}(Q)$ è la **funzione di transizione**, con l'insieme dei possibili stati successori
>4. $\color{#61AFEF}q_0\in Q$ è lo **stato iniziale**
>5. $\color{#61AFEF}F\subseteq Q$ è l'**insieme degli stati accettanti**

Dove:
- $\mathcal{P}(Q)$ è l'[[006 Insieme delle Parti|Insieme Potenza]] di $Q$
- $\Sigma_\epsilon$ è l'alfabeto contenente anche la stringa vuota, ovvero $\Sigma\cup \{\epsilon\}$

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240320122243.png|600]]
>
>La descrizione formale di $N_1$ è $(Q,\Sigma,\delta,q_1,F)$, dove:
>- $Q=\{q_1,q_2,q_3,q_4\}$
>- $\Sigma=\{0,1\}$
>- $\delta$ è data dalla seguente tabella
>
>
>
>|       | 0           | 1              | $\epsilon$  |
>| ----- | ----------- | -------------- | ----------- |
>| $q_1$ | $\{q_1\}$   | $\{q_1, q_2\}$ | $\emptyset$ |
>| $q_2$ | $\{q_3\}$   | $\emptyset$    | $\{q_3\}$   |
>| $q_3$ | $\emptyset$ | $\{q_4\}$      | $\emptyset$ |
>| $q_4$ | $\{q_4\}$   | $\{q_4\}$      | $\emptyset$ |
>
>- $q_1$ è lo stato iniziale
>- $F=\{q_4\}$



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
- [lydia - Video](https://www.youtube.com/watch?v=W8Uu0inPmU8&list=PLhqug0UEsC-IDomfNsn8e3neoy34o8oye&index=4)