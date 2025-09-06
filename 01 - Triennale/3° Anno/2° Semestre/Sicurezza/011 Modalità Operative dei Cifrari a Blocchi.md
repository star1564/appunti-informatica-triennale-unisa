---
aliases: 
  - ECB
  - CBC
  - CFB
  - OFB
  - CTR
tags:
  - corsi/informatica/sicurezza
paragrafo: Encryption Standards
cssclasses:
  - 
---
Una **Modalità Operativa dei [[008 Cifrari a Blocchi|cifrari a blocchi]]** è una serie di procedimenti standard di un algoritmo che permette di *aumentare la dimensione del testo da cifrare* (ad esempio, più di 64 bit per il [[010 Data Encryption Standard|DES]]) oppure per migliorare la sicurezza.

## Electronic Codebook Chaining (ECB)
>La modalità **ECB** è la più semplice. Il testo in chiaro viene gestito *64 bit per volta*, dove ognuno dei blocchi di 64 bit viene cifrato con la stessa chiave. 

Per testi in chiaro più lunghi di 64 bit, lo si *suddivide in blocchi di 64 bit*, utilizzando, se necessario, bit di riempimento nell'ultimo blocco.

![[Pasted image 20240611165017.png]]

Per una data chiave, esiste un unico testo cifrato per ogni blocco di testo in chiaro di 64 bit.

> [!success] Vantaggi
>- È veloce
>- La cifratura e la decifratura in modalità ECB è [[Parallelismo|parallelizzabile]]
>- Gli errori rimangono localizzati, quindi non si ha propagazione di errore su diversi blocchi

> [!failure] Svantaggi
>- Non c'è una dipendenza tra blocchi, poiché un attaccante potrebbe invertire dei blocchi e la vittima non se ne accorgerebbe (attacchi di sostituzione)
>- Usa una chiave fissa che quindi può essere predisposta alla [[003 Crittografia#^20ecc3|Crittoanalisi]]
>- Non è sicuro contro attacchi di [[003 Crittografia#^9a60da|Brute Force]]

## Cipher block chaining (CBC)
Per superare i limiti di sicurezza di ECB, è necessario l'utilizzo di una tecnica in cui lo stesso blocco di testo in chiaro, se ripetuto, produce *blocchi di testo cifrato differenti*. 

Questo è ciò che accade con la modalità **CBC**, in cui l'input dell'algoritmo è il risultato dello [[029 XOR (AdE)|XOR]] tra il blocco di testo in chiaro corrente e il blocco di testo cifrato precedente. Per ciascun blocco viene utilizzata la stessa chiave.  

![[Pasted image 20240611165602.png]]

Il vettore di inizializzazione $IV$ è di solito pubblico, ma potrebbe anche essere scelto a caso.

In fase di decifrazione, ciascun blocco di testo cifrato passa attraverso lo stesso algoritmo, ma il risultato subisce uno XOR con il blocco di testo cifrato precedente, per produrre il blocco di testo in chiaro.

![[Pasted image 20240611165740.png]]

> [!success] Vantaggi
> - La decifrazione è parallelizzabile
> - C'è una dipendenza tra blocchi
> - Non è vulnerabile ad attacchi di sostituzione

> [!failure] Svantaggi
> - La cifratura non è parallelizzabile
> - Gli errori possono essere propagati

### Ciphertext Stealing
Metodo per *evitare una espansione del testo cifrato* quando la sua lunghezza non è multipla del blocco del cifrario. Questo significa che si possono ottenere testi cifrati che hanno la stessa lunghezza del testo in chiaro.

Può essere applicato al CBC.

## Cipher feedback (CFB)

La modalità **CFB** è stata ideata per convertire idealmente una cifratura a blocchi in una [[014 Cifrari a Stream|cifratura a stream]].

![[Pasted image 20240611170735.png]]

Ad ogni passo viene eseguito uno [[051 Istruzioni Logiche MIPS#OPERAZIONI DI SHIFT|shift a sinistra]] di $j$ bit. Alla prima esecuzione, si utilizza $IV$.

![[Pasted image 20240611171054.png]]

> [!success] Vantaggi
> - La decifrazione è parallelizzabile

> [!failure] Svantaggi
>- La cifratura non è parallelizzabile
>- Gli errori possono essere propagati


## Output feedback (OFB)
La modalità **OFB** è molto simile alla CFB.

![[Pasted image 20240611171423.png]]

> [!success] Vantaggi
> - Non propaga gli errori di trasmissione dei bit

> [!failure] Svantaggi
> - È più vulnerabile a un attacco a modifica del flusso dei messaggi


## Counter (CTR)
Nel **Counter** viene utilizzato un contatore *corrispondente alle dimensioni del blocco* di testo in chiaro. 
Il requisito essenziale è che il suo valore sia differente per ciascun blocco da cifrare. In genere viene inizializzato con un determinato valore e poi incrementato di un'unità per ogni blocco successivo.

![[Pasted image 20240611171714.png]]

Per la cifratura, il contatore viene crittografato e poi si applica uno XOR col blocco di testo in chiaro per produrre il blocco di testo cifrato.  
Per la decifratura si utilizza la stessa sequenza di valori del contatore ai quali si applica lo XOR con i blocchi di testo cifrato.

> [!success] Vantaggi
>- Efficienza dell'hardware e software
>- Pre-elaborazioni
>- Accesso casuale
>- Sicuro
>- Semplice


___
[[000 Indice Sicurezza|↖ Ritorna all'indice ↖]]

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
- https://it.wikipedia.org/wiki/Modalit%C3%A0_di_funzionamento_dei_cifrari_a_blocchi