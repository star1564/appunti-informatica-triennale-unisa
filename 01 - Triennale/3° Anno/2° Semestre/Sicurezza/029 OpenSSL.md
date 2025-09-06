---
aliases: 
tags:
  - corsi/informatica/sicurezza
paragrafo: SSL/TLS
cssclasses:
  - 
---
>[OpenSSL](https://www.openssl.org/) è un'implementazione open source dei protocolli [[028 SSL e TLS|SSL e TLS]].
>Nei diversi linguaggi di programmazione sono disponibili procedure che permettono di accedere alle funzioni della libreria OpenSSL. Le librerie di base eseguono le funzioni crittografiche principali.

> [!summary] Comando
>Mediante questo comando possono essere accedute da command-line tutte le funzionalità offerte da OpenSSL.
>```shell
>openssl [comandi] [parametri]
>```

I comandi `s_client` ed `s_server` sono i principali strumenti forniti per il *debugging* di applicazioni Client/Server che utilizzano SSL/TLS. Tali comandi:
- possono essere *eseguiti indipendentemente* l'uno dall'altro
- sono *configurabili* mediante opportuni parametri

## Il comando s_client
Questo comando permette di realizzare tutte le funzioni di un semplice Client SSL/TLS.
Può essere usato per *connettersi ad un Server* che supporta tali protocolli.

È utile soprattutto come strumento di debugging per la creazione e la configurazione di Server SSL/TLS.

> [!summary]+ Comando
>```shell
>openssl s_client [args]
>```
>- ***`-connect [host:port]`*** - permette di connettersi ad un server
>- ***`-starttls [protocollo]`*** - permette di specificare quale protocollo si intende utilizzare sull'SSL/TLS (come [[037 SMTP|smtp]], ftp, mysql, ecc...)
>- `-CApath [dir]` - la directory con i [[026 Certificati|certificati]] delle CA
>- `-CAfile [file]` - il file della CA ulteriori informazioni per il debug
>- `-cipher [cs]` - specifica le [[028 SSL e TLS#Ciphersuite|Ciphersuite]]
>- `-verify [arg]` - imposta la verifica
>- `-cert [arg]` - certificato da usare, in formato `.pem`
>- `-key [arg]` - chiave privata relativa al certificato usato
>- `-msg` - mostra i messaggi del protocollo
>- `-showcerts` - mostra tutti i certificati presenti nella _certificate chain_
>- `-ssl(vers)`, `-tls(vers)` - richiedono l'uso delle versioni dei protocolli SSL o TLS specificati (sostituire `(vers)` con la versione)
>- `-no_ssl(vers)`, `-no_tls(vers)` - disabilitano l'uso delle versioni dei protocolli SSL o TLS specificati (sostituire `(vers)` con la versione)

Il Certificate Chain rappresenta la gerarchia di tutti i certificati, specificando il subject (`s`) e l'issuer (`i`), dove il primo certificato (0) è quello foglia, mentre l'utlimo è il root che può puntare a qualche *Certificato Root* che non è presente nella chain (oppure punta a se stesso se è un certificato self-signed).

> [!example]- <font color="orange">Esempio</font>
>```shell
>openssl s_client -msg -connect smtp.unipi.it:587 -starttls smtp
>```
>
>![[Pasted image 20240615171420.png|700]]
>![[Pasted image 20240615171432.png|700]]
>![[Pasted image 20240615171710.png|700]]
>![[Pasted image 20240615171757.png|700]]

## Il comando s_server
Questo comando permette di realizzare tutte le funzioni di un semplice Server SSL/TLS.
Può essere usato per *accettare connessioni entranti* da parte di Client.

> [!summary]+ Comando
>```shell
>openssl s_server [args]
>```
>
>- ***`-accept [porta]`*** - accetta richieste sulla porta specificata (default 4433)
>- ***`-verify [arg]`*** - richiede l'autenticazione dal Client
>- ***`-cert [file]`*** - certificato del Server
>- `-key [file]` - il file con la chiave privata del Server
>- `-debug` - visualizza ulteriori informazioni per il debug
>- `-CApath [dir]` - la directory con i certificati delle CA
>- `-CAfile [file]` - il file del certificato della CA
>- `-cipher [cs]` - specifica le Ciphersuite
>- `-ssl(vers)`, `-tls(vers)` - richiedono l'uso delle versioni dei protocolli SSL o TLS specificati (sostituire `(vers)` con la versione)
>- `-no_ssl(vers)`, `-no_tls(vers)` - disabilitano l'uso delle versioni dei protocolli SSL o TLS specificati (sostituire `(vers)` con la versione)
>- `-www` - fornisce una risposta a `GET` con una pagina HTML di prova

> [!example]- <font color="orange">Esempio</font>
>```shell
>openssl s_server -accept 12345 -cert server-cert.pem -key private/server-key.pem
>```
>![[Pasted image 20240615172839.png|700]]
>
>Mediante questo comando è possibile connettersi al Server sulla porta 12345
>
>```shell
>openssl s_client -msg -connect localhost:12345
>```
>
>Alla ricezione di una richiesta di connessione effettuata mediante l's_client, si avrà una situazione del genere:
>
>![[Pasted image 20240615173040.png|700]]
>
>Si può anche aggiungere `-www` per creare un semplice [[011 Web Dinamico|Web Server]] a cui connettersi tramite browser.
>
>![[Pasted image 20240615173244.png|600]]

## Formati di Codifica in OpenSSL
Ci sono due principali formati:
- **Distinguished Encoding Rules** (*DER*)

- **Privacy Enhanced Mail** (*PEM*)
	- Formato di default usato da OpenSSL
	- Usato per codificare in formato testuale (Base64) oggetti codificati tramite DER
	-  Può contenere commenti
	-  Ogni file può contenere oggetti multipli codificati in PEM

Il PEM ha la seguente sintassi
```
---BEGIN keyword---
Base 64 encoding
---END keyword---
```

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
- https://it.wikipedia.org/wiki/OpenSSL
- https://www.misterpki.com/openssl-s-client/