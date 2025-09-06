---
aliases:
  - Autorità di Certificazione (CA)
tags:
  - corsi/informatica/sicurezza
paragrafo: Certificati e PKI
cssclasses:
  - 
---
>Un **Certificato Digitale** (*public key certificate*) è usato per *provare la validità di una [[005 Cifrari Asimmetrici#^29e17a|Chiave Pubblica]]*. 
>I certificati digitali sono emessi da *autorità di certificazione* (CA) all'interno di una [[027 Public-Key Infrastructure|PKI]].

Un certificato è formato da:
- Una *chiave pubblica*
- Un numero di serie
- Un *identificatore* del proprietario della chiave
- La *[[022 Firme Digitali|firma]] di una terza parte fidata* (la CA che ha rilasciato il certificato), con la [[005 Cifrari Asimmetrici|Chiave Privata]] della CA
- Altre informazioni sul certificato, come la scadenza

> [!question] Code Signing
> I certificati sono usati anche per il codice nel Code Signing, per garantire l'[[001 Introduzione alla Sicurezza sui Dati#^9a165d|integrità]] e la provenienza del codice.


### Autorità di Certificazione (CA)
Un utente può presentare la propria chiave pubblica all'autorità in modo sicuro e ottenere un certificato. L'utente può quindi pubblicare il certificato. Chiunque abbia bisogno della chiave pubblica di questo utente *può ottenere il certificato e verificarne la validità* attraverso la firma di fiducia allegata. 

![[Pasted image 20240614154828.png|600]]

> [!question]- Due certificati emessi dalla stessa CA *devono sempre* avere numeri di serie diversi


Un partecipante può anche trasmettere le sue informazioni sulla chiave a un altro partecipante trasmettendo il suo certificato. Gli altri partecipanti possono verificare che il certificato sia stato creato dall'autorità.

![[Pasted image 20240614154935.png|700]]

> [!question]+ Cosa conserva una CA
> - Le copie di tutti i certificati che ha emesso
> - Le copie di tutti i certificati che in corso di validità
> - Le copie di tutti i certificati revocati



### Chain of Trust
È il processo attraverso il quale viene *stabilita* la validità di un certificato digitale e, di conseguenza, *la fiducia nell'identità* di una parte comunicante su una rete non sicura come Internet.

![[Pasted image 20240313160549.png|500]]

### Revoca di Certificati
Quando una chiave privata *viene compromessa* (oppure quando le informazioni del certificato non sono più valide), l'utente può richiedere la *revoca* del certificato inerente alla chiave pubblica.

#### CRL
Esiste un file pubblico di chiavi revocate chiamata **Certificate Revocation List (CRL)**. Questa lista viene firmata dalla CA contenente:
- numeri seriali dei certificati emessi *revocati* ma *non ancora scaduti*
- quando è avvenuta la revoca
- altro

> [!question] Senza l'utilizzo di una CRL, l'unica entità a conoscenza della revoca di un determinato certificato è la CA che ha effettuato la revoca

### Gerarchia di CA
Come per un [[036 DNS|DNS]], anche qui è presente una gerarchia di CA. Ogni CA gestisce i propri certificati.

![[Pasted image 20240614160644.png|500]]

Due diverse CA possono anche *comunicare* tra di loro in caso un entità non è in grado di verificare un certificato con chiave pubblica sconosciuta.

![[Pasted image 20240614160805.png|700]]

### Uso dei Certificati sull'Internet
Sull'Internet i certificati sono usati comunemente per accedere a siti web tramite [[005 Sicurezza per HTTP|HTTPS]].

## Standard dei Certificati X.509
Questo standard è quello più usati ed è composto da vari campi.

![[Pasted image 20240614155336.png|500]]

### Versione 1 
- `Version` - Un intero che definisce il numero della *versione del certificato*. Ci sono tre versioni:
	- 1, default
	- 2, se presente `Issuer unique identifier` oppure `Subject unique identifier`
	- 3 se ci sono `Estensioni`
- `Serial Number` - Un intero che rappresenta l'*identificatore* unico rilasciato dalla CA
- `Signature Algorithm ID` - L'*algoritmo usato* dalla CA per *firmare il certificato*
- `Issuer Name` - Il nome distinto della *CA* che ha firmato il certificato
- `Validity Period` - Data di *creazione e di scadenza* del certificato
- `Subject Name` - Il nome distinto dell'utente (soggetto del certificato), cioè chi conosce la chiave privata corrispondente a quella pubblica rilasciata con questo certificato
- `Subject Pubblic Key Info` - È la chiave pubblica posseduta dal soggetto
- ***`Firma di tutti i campi`*** - Firma dell'[[019 Algoritmi di Hashing Crittografici|hash]] di tutti i campi

### Versione 2
Opzionali.

- `Issuer unique identifier` - Un identificatore unico che rappresenta la CA che ha rilasciato il certificato, in caso l'`Issuer Name` non sia distinto
- `Issuer unique identifier` - Un identificatore unico che rappresenta il soggetto che possiede la chiave pubblica, in caso il `Subject Name` non sia distinto

### Versione 3
Opzionale.

- `Extension` - Un'insieme di estensioni standard per certificati su Internet



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