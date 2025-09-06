---
tags:
  - corsi/informatica/reti_calcolatori
paragrafo: Livello Rete
cssclasses:
  - 
---
>L'**Internet Protocol** (*IPv4*) è un protocollo di comunicazione fondamentale utilizzato per il trasferimento di dati nel [[022 Livello di Rete|livello di rete]]. Ha la funzione di recapitare [[022 Livello di Rete#^9b21d6|datagrammi]] *dalla sorgente alla destinazione* tramite reti interconnesse tra loro.

L'IP opera su un'infrastruttura di rete a *commutazione di pacchetto*, in cui i dati vengono divisi in pacchetti più piccoli per la trasmissione. Questi pacchetti IP contengono l'[[Indirizzo IP]] di origine e destinazione, che consentono ai dispositivi di comunicare tra loro all'interno di una rete o attraverso Internet.

## Specifiche tecniche di un Indirizzo IP
L'indirizzo IP è formato da *32 bit* rappresentati da *quattro numeri decimali* separati da un punto che possono essere un valore nel range $[0, 255]$.

$$XX.XX.XX.XX$$

Ogni indirizzo IP contiene una parte che *specifica la rete* (prefisso) e una parte che *identifica l'host* (suffisso) all'interno di una rete.

Sono raggruppati in **Classi**:
- **Classe A**: il primo campo assume un valore nel range $\color{#61AFEF}[0,127]$. Il *primo* campo è dedicato al prefisso, il resto al suffisso. La sequenza di bit comincia con $\color{#CC241D}0$;
- **Classe B**: il primo campo assume un valore nel range $\color{#61AFEF}[128,191]$. I primi *due* campi sono dedicati al prefisso, il resto al suffisso. La sequenza di bit comincia con $\color{#CC241D}10$;
- **Classe C**: il primo campo assume un valore nel range $\color{#61AFEF}[192,223]$. I primi *tre* campi sono dedicati al prefisso, il resto al suffisso. La sequenza di bit comincia con $\color{#CC241D}110$;
- **Classe D**: il primo campo assume un valore nel range $\color{#61AFEF}[224,239]$. Sono indirizzi dedicati al *multicasting*. La sequenza di bit comincia con $\color{#CC241D}1110$;
- **Classe E**: il primo campo assume un valore nel range $\color{#61AFEF}[240,255]$. Sono indirizzi ad utilizzi *sperimentali*. La sequenza di bit comincia con $\color{#CC241D}1111$.

![[Pasted image 20230526115134.png|500]]

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20230526115215.png]]

L'indirizzo $127.0.0.0$ indica l'interfaccia di loopback, la quale identifica la macchina locale detta *localhost*.

### Campi con valore 0 e 1
- L'indirizzo IP contenente tutti i bit pari a 0 *nel campo di host* (suffisso) indica la rete specificata nel campo di rete.
- L'indirizzo IP contenente tutti i bit pari a 0 *nel campo di rete* (prefisso) indica "questa rete".
- L'indirizzo IP contenente tutti i bit pari a 0 *sia nel campo di rete che nel campo di host* indica "questo host di questa rete".

- L'indirizzo IP contenente tutti i bit pari a 1 *sia nel campo di rete che nel campo di host* indica l'indirizzo di [[002 Rappresentazione di Reti#Broadcast|broadcast]] della rete locale, quindi viene utilizzato per mandare un pacchetto IP ad ogni host sulla propria rete.
- L'indirizzo IP contenente tutti i bit pari a 1 *nel campo di host* (suffisso) indica il broadcast nella rete specificata nel campo rete, quindi viene utilizzato per mandare un pacchetto IP ad ogni host appartenente ad una certa rete remota.

## Pacchetto IP
Il pacchetto IP è costituito da un *header* di lunghezza fissa di 20 byte, più una parte opzionale (fino a 40 byte).

![[Pasted image 20230529094043.png|700]]

## Assegnamento IP
Affinché tutto funzioni correttamente, gli indirizzi IP devono essere assegnati da una autorità centrale che garantisca l'unicità delle assegnazioni (siccome ogni indirizzo IP deve essere unico in tutta la rete). Per internet gli indirizzi sono assegnati dalla **ICANN**, la quale ha poi delegato organizzazioni regionali assegnando loro gruppi di indirizzi da riassegnare al loro interno.

Lo spazio di assegnamento equivale a circa due miliardi di indirizzi, ma con il passare degli anni ci si è accorti di una certa carenza di indirizzi. Per risolvere tale problema, è stata sviluppata una tecnica detta [[024 Network Mask e Subnetting|subnetting]], la quale permette di suddividere un campo di indirizzi in gruppi più piccoli, trattando un sottogruppo come se fosse una rete a sé stante.

> [!example]- <font color="orange">Esempio</font>
> Un campus ha rete associata 100.0.0.0 (classe A) e si vuole suddividere il suo campo di indirizzi: è possibile associare 100.1.0.0 ad un dipartimento, 100.2.0.0 ad un altro dipartimento e così via, in modo da considerare le reti dei dipartimenti come reti a sé stanti e di classe più piccola, il tale classe B. Qualsiasi rete può essere subnettata.

## CIDR
Per ovviare al problema del numero crescente di indirizzi, is pensò di *abbandonare le classi di rete* consentendo di avere un sistema avente una migliore gestione evitando sprechi di indirizzi, ovvero il **Classless InterDomain Routing** (**CIDR**).

Questo sistema permette la *suddivisione dell'indirizzo IP in prefisso e suffisso senza la suddivisione in classi*. 

> [!error] Problema del CIDR
> L'aumento delle reti indirizzabili può far esplodere le dimensioni della [[022 Livello di Rete#^17460a|tabella di routing]].

Per risolvere questo problema, gli indirizzi vengono assegnati alle varie organizzazioni regionali e locali che risultano verso l'esterno come una sola rete.

![[Pasted image 20230529091750.png|500]]


___
[[000 Indice RC|↖ Ritorna all'indice ↖]]
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