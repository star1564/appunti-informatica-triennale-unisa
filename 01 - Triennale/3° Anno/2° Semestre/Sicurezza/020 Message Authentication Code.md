---
aliases:
  - MAC (sicurezza)
  - CBC-MAC
  - CMAC
  - HMAC
  - CCM
tags:
  - corsi/informatica/sicurezza
paragrafo: Algoritmi di Crittografia
cssclasses:
  - 
---
>Il **Message Authentication Code (MAC)** è un metodo di sicurezza crittografica utilizzato per *verificare l'[[001 Introduzione alla Sicurezza sui Dati#^9a165d|integrità]] e l'[[001 Introduzione alla Sicurezza sui Dati#^61b340|autenticità]] di un messaggio*. 
>È una funzione che prende in input un messaggio e una chiave segreta *condivisa* e produce un codice (il MAC) che viene trasmesso insieme al messaggio. 

> [!warning] Da non confondere con il [[MAC (Indirizzo e Sottolivello)]]

![[Pasted image 20240616083047.png|600]]

Il destinatario confronta il *MAC calcolato da se* con il *MAC ricevuto*. Se i due MAC coincidono, il destinatario può essere sicuro che il messaggio non è stato alterato e proviene dal mittente legittimo. Se i MAC non coincidono, il destinatario sa che il messaggio è stato alterato o che proviene da un mittente non autorizzato.

> [!note] Il MAC non fornisce [[001 Introduzione alla Sicurezza sui Dati#^ff3019|confidenzialità]]
> Se si vuole confidenzialità, si può cifrare prima o dopo con una chiave diversa condivisa.

## Sicurezza

### Tipi di attacchi
Qui, Oscar denota un attaccante, mentre Alice e Bob i due utenti:
- **Known Message Attack** - Oscar conosce una lista di messaggi ed i relativi MAC
- **Chosen Message Attack** - Oscar sceglie dei messaggi e chiede ad Alice (o Bob) di computarne i MAC
- **Adaptive Chosen Message Attack** - Come nel CMA, però le scelte dipendono dalle risposte precedenti

Una volta che Oscar ha avuto successo nel rompere lo schema del MAC, lui può:
- **Total Break** - Determinare la chiave $K$ del MAC
- **Selective forgery** - Dato un messaggio $M$, può determinarne $y$ tale che $y=MAC(M,K)$
- **Existential forgery** - Può determinare una coppia $(M,y)$ tale che $y=MAC(M,K)$

### Brute force
Questo approccio si basa sulla prova di tutte le combinazioni possibili di chiavi segrete fino a quando non si trova una chiave che produce il MAC corretto per un determinato messaggio.

Consideriamo le coppie $(M_i,y_i)$, dove $M_i$​ è il messaggio e $y_i=MAC(M_i,K)$ è il MAC generato con la chiave segreta $K$. Supponiamo che la lunghezza della chiave sia $k$ bit e che il MAC sia $n$ bit.

1. $\color{#61AFEF}i=1$. Prova tutte le $2^k$ chiavi sulla coppia $\color{#61AFEF}(M_1, y_1)$:
    - Dato il messaggio $M_1$​ e il MAC $y_1$​, si provano tutte le $2^k$ possibili chiavi $K$.
    - Per ogni chiave, si calcola $MAC(M_1, K)$ e si confronta con $y_1$.
    - Statisticamente, ci si aspetta che circa $2^{k-n}$ chiavi producano un match con $y_1$​, poiché ci sono $2^n$ possibili valori di MAC.
2. $\color{#61AFEF}i=2$. Prova le $2^{k-n}$ chiavi precedenti sulla coppia $(M_2, y_2)$:
    - Si prendono le $2^{k-n}$ chiavi che hanno prodotto un match con $y_1$​ e si ripete il processo con la coppia $(M_2, y_2)$.
    - Per ogni chiave, si calcola $MAC(M_2, K)$ e si confronta con $y_2$.
    - Ci si aspetta che circa $2^{k-2n}$ chiavi producano un match con $y_2$​.
3. *Ripetizione del processo*:
    - Si continua a ripetere il processo con le successive coppie $(M_i, y_i)$.
    - Dopo $i$ round, il numero di chiavi candidate si riduce a $2^{k-in}$.

In generale, se $k=\beta\cdot n$ (ovvero la lunghezza della chiave $k$ è un multiplo della lunghezza del MAC $n$), allora *sono necessari circa $\beta$ round* per ridurre il numero di chiavi candidate a 1, ossia *per determinare la chiave $K$ corretta*.

Lo sforzo computazionale totale per una ricerca esaustiva può essere approssimato come $\min(2^k, 2^n)$:
- $\color{#CC241D}2^k$: Se la lunghezza della chiave $k$ è maggiore della lunghezza del MAC $n$, allora è necessario provare tutte le $2^k$ possibili chiavi per determinare quella corretta.
- $\color{#CC241D}2^n$: Se la lunghezza del MAC $n$ è maggiore della lunghezza della chiave $k$, allora ci si aspetta che la ricerca si concluda prima, poiché ci sono meno possibili valori di MAC da confrontare.


## Tipi di MAC
### CBC-MAC
>Il **Cipher Block Chaining** Message Authentication Code **(CBC-MAC)** utilizza una [[008 Cifrari a Blocchi|cifratura a blocchi]] in modalità [[011 Modalità Operative dei Cifrari a Blocchi#Cipher block chaining (CBC)|CBC]]. 
>Il testo *viene diviso in blocchi da 64 bit* attraverso il [[010 Data Encryption Standard|DES]].

![[Pasted image 20240616091432.png|650]]

### CMAC
>Il **Cipher-based** Message Authentication Code **(CMAC)** è basato su cifrature a blocchi con *miglioramenti rispetto a CBC-MAC* per maggiore sicurezza.
>Il testo *viene diviso in blocchi da 128 bit* attraverso l'[[013 Advanced Encryption Standard|AES]].

![[Pasted image 20240616091843.png|700]]

### HMAC
>L'**Hash-based** Message Authentication Code **(HMAC)** utilizza una *[[019 Algoritmi di Hashing Crittografici|Funzione Hash Crittografica]]* combinata con una *chiave segreta*.

Il messaggio viene suddiviso in $L$ blocchi da $b>n$ bit ciascuno $(b=512)$.

Se la lunghezza della chiave segreta è:
- minore di $b$, si fa padding con $0\dots0$
- maggiore di $b$, calcola $H(K)$ di $n$ bit e si fa padding con $0\dots0$

![[Pasted image 20240616092312.png|700]]

#### Output Troncato
Si può anche usare solo i primi $t$ bit dell'hash, come in HMAC-SHA1-80, dove vengono usati solo i primi 80 bit dei 160 bit.

#### Sicurezza HMAC
Questa dipende dalle proprietà della funzione hash usata da HMAC.

### CCM
>Il **Counter with Cipher Block Chaining** Message Authentication Code **(CCM)** utilizza una cifratura a blocchi in modalità [[011 Modalità Operative dei Cifrari a Blocchi#Counter (CTR)|CTR]]. Utilizza l'AES.

#### Autenticazione
![[Pasted image 20240616093559.png|700]]

#### Cifratura
![[Pasted image 20240616093633.png|700]]


---

> [!example]- <font color="orange">Esempio</font>
>1. Alice:
>	- Messaggio: "Ciao Bob"
>	- Chiave segreta: "chiave123"
>	- Algoritmo MAC: HMAC-SHA256
>	- Calcola il MAC e ottiene "5d41402abc4b2a76b9719d911017c592"
>	- Invia il messaggio "Ciao Bob" e il MAC "5d41402abc4b2a76b9719d911017c592" a Bob
>
>2. Bob:
>	 - Riceve il messaggio "Ciao Bob" e il MAC "5d41402abc4b2a76b9719d911017c592"
>	 - Utilizza la chiave segreta "chiave123" e l'algoritmo HMAC-SHA256 per calcolare il MAC del messaggio ricevuto
>	 - Ottiene "5d41402abc4b2a76b9719d911017c592"
>	 - Confronta i MAC e verifica che coincidono, confermando che il messaggio è autentico e non è stato alterato
>
>Il MAC è uno strumento fondamentale nella crittografia moderna, utilizzato in numerosi protocolli di sicurezza per garantire comunicazioni sicure.


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