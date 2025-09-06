---
aliases: 
  - A5
  - RC4
  - Salsa20
  - ChaCha20
tags:
  - corsi/informatica/sicurezza
paragrafo: Cifrari a Stream
cssclasses:
  - 
---
## A5
>L'**A5** è uno [[014 Cifrari a Stream|Stream Cipher]] usato per cifrare la comunicazione via etere sotto forma di frame di 114 bit. Ogni frame è identificato da 22 bit.
>È composto da tre [[015 Linear Feedback Shift Register|LSFR]] di grado 19, 22, 23.

Utilizza una keystream per ciascun frame, dove la chiave si trova nella memoria della SIM (Subscriber Identity Module).


![[Pasted image 20240612145247.png|700]]


Ad ogni passo ciascun registro viene shiftato *se il suo bit centrale concorda con la maggioranza* dei bit centrali dei tre registri. In ogni round vengono shiftati almeno due registri.

![[Pasted image 20240612145802.png|700]]

I registri vengono inizializzati con una combinazione non lineare di $K_c$ e i 22 bit del numero di frame. 
In totale, sono utilizzati 228 bit della keystream.

![[Pasted image 20240612145930.png|700]]


Questo rende la complessità di un attacco [[003 Crittografia#^9a60da|Brute Force]] $2^{64}$. Però in genere, la chiave $K_c$ ha solo 54 bit, dove gli ultimi 10 bit sono posti a 0, quindi ha complessità $2^{54}$.

## RC4
>L'**RC4** è uno [[014 Cifrari a Stream|Stream Cipher]] con una dimensione della *chiave variabile da 1 a 256 byte* che esegue operazioni orientate ai byte.
>Questo genera una keystream con periodo maggiore di $10^{100}$.

1. Viene definita una chiave con una lunghezza variabile tra 1 e 256 byte.

![[Pasted image 20240612152038.png]]


2. Si crea una tabella di stato $S$ con 256 elementi, ognuno inizializzato con un valore distinto da 0 a 255.

![[Pasted image 20240612152019.png]]

```C
for (int i = 0; i < 255; i++)
	S[i] = i
```

3. La chiave segreta viene utilizzata per permutare la tabella di stato in modo caotico. Questo lo si ottiene scambiando ciascun `S[i]` con un `S[j]`, dipendente da `S[i]` e da `K[i mod h]`.

![[Pasted image 20240612152409.png]]

```C
int j = 0, i = 0;
for (i = 0; i < 255; i++) {
	j = (j + S[i] + K[i mod h]) mod 256;
	swap(S[i], S[j]);
}
```

### Generazione della Keystream

Il vettore $S$ è usato per generare la keystream, effettuando lo [[029 XOR (AdE)|XOR]] del byte $k$ con il prossimo byte del testo in chiaro.

![[Pasted image 20240612152612.png]]

```C
int i = 0, j = 0;

while (true) {
	i = i+1 mod 256;
	j = (j+S[i]) mod 256;
	swap(S[i], S[j]);
	t = (S[i] + S[j]) mod 256;
	k = S[t];
}
```

## Salsa20
>Il **Salsa20** è uno [[014 Cifrari a Stream|Stream Cipher]] con chiave da 128 o 256 bit basato su operazioni *add-rotate-XOR (ARX)* su 32 bit.

1. Viene definita una chiave segreta a 256 bit (o 128 bit) e un nonce a 64 bit (un numero generato a caso che viene *usato una sola volta*).
2. Lo stato iniziale dell'algoritmo è rappresentato da una matrice di 16 parole a 32 bit.

![[Pasted image 20240612153058.png]]

3. La chiave segreta, il nonce e un contatore vengono utilizzati per inizializzare la matrice di stato. Con una costante sulla diagonale scritta in ASCII "expand 32-byte k".

![[Pasted image 20240612153210.png]]

![[Pasted image 20240612153304.png]]

4. Viene eseguita la **Core function**, la funzione principale di Salsa20 che esegue una serie di operazioni di addizione, XOR e rotazione bit su righe e colonne della matrice di stato. La core function viene eseguita per *20 round*.

5. Il keystream viene generato estraendo le word dalla matrice di stato. Questo è continuo e può essere generato per qualsiasi lunghezza.

## ChaCha20
>Il **ChaCha20** è un derivato dell'algoritmo Salsa20, con alcune modifiche per migliorare la sicurezza e le prestazioni.


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