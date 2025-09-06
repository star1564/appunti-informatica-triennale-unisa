---
aliases:
  - Ciphersuite
tags:
  - corsi/informatica/sicurezza
paragrafo: SSL/TLS
cssclasses:
  - 
---
>Il **Secure Socket Layer (SSL)** è un [[Protocollo|protocollo]] che offre meccanismi di sicurezza alle applicazioni che usano il [[030 TCP|protocollo TCP]]. Ad esempio rende [[005 Sicurezza per HTTP|sicuro il protocollo HTTP]]. 
>Usa algoritmi di crittografia a [[005 Cifrari Asimmetrici|chiave pubblica]].

![[Pasted image 20240615142121.png|700]]

> [!info] SSL è deprecato

>Il **Transport Layer Security (TLS)** è *il diretto aggiornamento di SSL*. 
>TLS v1.3 è la versione attuale.

### Caratteristiche di SSL/TLS
I protocolli SSL e TLS:
- Forniscono l'[[001 Introduzione alla Sicurezza sui Dati#^61b340|autenticazione]] per applicazioni Server e Client
- *Cifrano i dati* prima di inviarli sul canale pubblico, garantendo la [[001 Introduzione alla Sicurezza sui Dati#^ff3019|confidenzialità]]
- Garantisce l'[[001 Introduzione alla Sicurezza sui Dati#^9a165d|integrità]] dell'informazione
- Le primitive crittografiche vengono [[018 Protocollo Diffie-Hellman|negoziate tra le parti]]

## Componenti di SSL/TLS

SSL/TLS operano tra il [[029 Livello di Trasporto|Livello di Trasporto]] (al di sopra di [[030 TCP|TCP]]) ed i [[005 Modello ISO-OSI#Livelli di Processo o Applicazione|livelli applicativi]] e sono composti da quattro protocolli.

![[Pasted image 20240615143832.png|700]]


### Handshake Protocol
Questo è un protocollo che permette alle parti di *negoziare* i diversi algoritmi e parametri necessari (**Ciphersuite**) per stabilire una comunicazione sicura. Qui può anche avvenire l'eventuale *autenticazione tra le parti*. Questo garantisce l'[[Interoperabilità]] tra le diverse implementazioni del protocollo.

> [!question]- Non impone alle parti l'esecuzione di un nuovo handshake



#### Ciphersuite
Una **Ciphersuite** è una *stringa* che rappresenta gli algoritmi ed i parametri scelti per la connessione. Definisce diversi schemi: ^6af453
- **accordo/scambio di chiavi** - stabilisce il modo in cui verranno scambiate le chiavi utilizzate dagli algoritmi a chiave simmetrica
	- [[017 RSA|RSA]], [[018 Protocollo Diffie-Hellman|DH]], ECDHE (variante del DH con curva elliptica)
- **autenticazione** (opzionale) - stabilisce in che modo verrà eseguita l'autenticazione del server e (se necessario quella del client)
	- RSA, DSA, ECDSA
- **cifratura simmetrica** - stabilisce quale algoritmo a chiave simmetrica verrà utilizzato per cifrare i dati
	- [[013 Advanced Encryption Standard|AES]], [[012 Cifratura Multipla del DES#Triplo DES|3DES]]
- **autenticazione del messaggio (MAC)** - stabilisce il metodo che la sessione utilizzerà per il controllo dell'integrita dei dati
	- [[019 Algoritmi di Hashing Crittografici|SHA]], [[019 Algoritmi di Hashing Crittografici|MD5]]



> [!example]- <font color="orange">Esempio</font>
>Ad esempio `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`:
>- `TLS` - protocollo usato
>- `ECDHE` - scambio di chiavi
>- `RSA` - autenticazione con public key
>- `AES_128_GCM`:
>	- `AES` - algoritmo per la cifratura simmetrica
>	- `128` - bit usati dalla chiave dell'algoritmo
>	- `GCM` - modalità dell'algoritmo
>- `SHA256` - algoritmo utilizzato per l'hashing

#### Procedimento
Dopo aver stabilito una connessione TCP con il [[034 Apertura e Chiusura connessione TCP|TCP Handshake]], verrà eseguito il **TLS Handshake**. Tutti i messaggi TLS hanno un numero che li identifica.

> [!question] Permette alle due parti di calcolare in maniera indipendente la stessa chiave di [[014 Session Tracking|sessione]]



![[Pasted image 20240617145540.png|900]]

##### Prima Fase (presentazione)
In questa fase si vuole calcolare un *Master Secret*, usato nell'[[018 Protocollo Diffie-Hellman|Accordo sulla Chiave in comune]].

1. **ClientHello** - è il messaggio iniziale inviato dal Client al Server:
	- `max_tls_client_version` - è la versione massima che può supportare il client
	- `random` - è un numero casuale di 28 byte per evitare attacchi di tipo [[002 Attacchi e Vulnerabilità#^159950|replay]] concatenato con un timestamp di 4 byte del Client
	- `session_id` - permette al Client di proporre al Server di riutilizzare una [[014 Session Tracking|sessione]] già stabilita in precedenza
	- `cipher_suites` - una lista di Ciphersuite supportate dal Client
	- `compression_methods` - una lista degli algoritmi di hashing supportati dal Client

2. **ServerHello** - il Server *sceglie tutti i parametri* in base alle capacità del Client e manda una risposta:
	- `server_certificate` (opzionale) - è il [[026 Certificati|certificato]] del Server contenente la sua chiave pubblica, utilizzata anche per cifrare il *pre-master secret*, un numero casuale generato dal Client che verrà usato dalle due parti per la generazione della chiave di sessione
	- `random` - è un numero casuale di 28 byte per evitare attacchi di tipo replay concatenato con un timestamp di 4 byte del Server
	- `session_id` (opzionale) - se lo stesso dell'ID indicato da `session_id` di ClientHello, il Server è d'accordo nel riutilizzare una vecchia sessione, altrimenti viene creata una nuova sessione
	- `server_key_exchange` (opzionale) - utilizzato per spedire una *chiave temporanea* per l'accordo tra le chiavi (prima parte dell'accordo)
	- `certificate_request` (opzionale) - è un messaggio tramite cui il Server richiede di autenticare il Client
	- `digital_sign` - è la [[022 Firme Digitali|firma digitale]] del server, usata per provare che il Server è effettivamente il mittente del messaggio

3. **ServerHelloDone** - indica la fine della fase di "presentazione" del Server

![[Pasted image 20240615160548.png|600]]

##### Seconda Fase
In questa fase viene calcolata una *Chiave di Sessione*.

4. **Trasmissione Client**:
	- `client_certificate` (opzionale) - viene spedito solo se il Server lo ha richiesto il certificato del Client con `certificate_request`
	- `client_key_exchange` - il pre-master secret viene cifrato con la chiave pubblica del Server e viene trasmesso a quest'ultimo (seconda parte dell'accordo)
	- `certificate_verify` (opzionale) - viene spedito solo se il Server ha richiesto l'autenticazione con `certificate_request`, contiene la firma di tutti i messaggi precedentemente scambiati, per provare che il Client è effettivamente il mittente del messaggio
	- `change_cipher_spec` - viene spedito *per confermare* al Server che tutti i messaggi che seguono *saranno protetti* usando le chiavi e gli algoritmi (ciphersuite) appena negoziati

5. **Finished (Client)**: è il primo messaggio sicuro, contiene *l'hash di tutti i messaggi scambiati* fino a quel momento ed è cifrato usando la chiave di sessione.

6. **Trasmissione Server**:
	- `change_cipher_spec` - viene spedito per confermare al Client che tutti i messaggi che seguono saranno protetti usando le chiavi e gli algoritmi (ciphersuite) appena negoziati

7. **Finished (Server)**: contiene l'hash di tutti i messaggi scambiati fino a quel momento ed è cifrato usando la chiave di sessione

> [!note]+ Messaggi `Finished`
> Dato che `Finished` rappresenta un riassunto della "conversazione", possono essere usati per vedere *se la comunicazione è compromessa*.
> 
> Infatti, se i due messaggi `Finished`:
> - sono **uguali**, la fase di handshake SSL/TLS è *avvenuta con successo*, quindi la chiave di sessione dal Client calcolata coincide con quella calcolata dal Server
> - sono **diversi**, significa che c'è stato un *errore imprevisto o un attacco*, quindi la connessione viene terminata immediatamente

![[Pasted image 20240615160618.png|600]]

### Record Protocol
Utilizza gli algoritmi e i parametri negoziati durante l'Handshake TLS e si occupa della *compressione*, del *MAC* e della *cifratura*.

![[Pasted image 20240615161859.png|600]]

> [!question]+ Cosa NON fa Record protocol
> - *non* garantisce l'autenticazione
> 	- *non* consente di ottenere la mutua autenticazione tra le parti
> - *non* garantisce l'interoperabilità tra le diverse implementazioni del protocollo SSL/TLS


### Alert Protocol
Notifica situazioni anomale o *segnala eventuali problemi* da una parte verso l'altra. Ci sono due livelli di severità:
- `warning`
- `fatal`

### Change Cipher Spec
È utilizzato per aggiornare la Ciphersuite in uso tra il Client ed il Server.

---
# 

> [!summary] Riassunto SSL/TLS
> - Consentono di ottenere i requisiti di autenticazione, confidenzialità ed integrità
> - Consentono alle parti di negoziare le primitive crittografiche da utilizzare per la sicurezza dell'informazione
> - Consente al Client di autenticare il Server, ed eventualmente il viceversa


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
- [TLS e SSL - PowerCert](https://www.youtube.com/watch?v=hExRDVZHhig)
- [TLS - Computerphile](https://www.youtube.com/watch?v=0TLDTodL7Lc?t=566)
- [TLS Handshake - Computerphile](https://www.youtube.com/watch?v=86cQJ0MMses)

