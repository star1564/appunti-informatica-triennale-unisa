---
aliases:
  - AES
  - Rijndael
tags:
  - corsi/informatica/sicurezza
paragrafo: Encryption Standards
cssclasses:
  - 
---
>L'**Advanced Encryption Standard (AES, Rijndael)** è un algoritmo a [[004 Cifrari Simmetrici|chiave simmetrica]] che ha sostituito il [[010 Data Encryption Standard|DES]]. Anche questo sfrutta la struttura ad iterazione con i round. 

Questo *NON è un [[009 Cifrari di Feistel|Cifrario di Feistel]]*, infatti opera sull'intero blocco in input di dati da *128 bit* e supporta chiavi di lunghezza pari a: 
- *128 bit* per 10 round
- *192 bit* per 12 round
- *256 bit* per 14 round

Quindi la sua complessità è al minimo $2^{128}$.

![[Pasted image 20240612104052.png]]

Tutte le operazioni usate dall'AES sono facilmente ed efficientemente implementabili sia su architetture ad 8 bit che a 32 bit.

## State
Il blocco di input da 128 bit (*16 byte*) viene rappresentato come una [[058 Matrici|Matrice]] di bytes $\color{#61AFEF}4\times 4$. Questo viene copiato in un array **State**, che viene modificato ad ogni round della crittografia. 

![[Pasted image 20240612105438.png|700]]

Le *colonne* di questa matrice vengono chiamate *word* e sono da 32 bit (4 byte).

![[Pasted image 20240612105010.png]]

Dopo l'ultimo round, State è copiato in una matrice di output.

![[Pasted image 20240612105600.png|700]]

### State per la chiave
Anche la chiave viene inserita in una matrice di 4 righe, ma il numero delle colonne *varia in base al numero di bit della chiave* (128 → 4, 192 → 6, 256 → 8).

## Operazioni su byte
Il *byte* (8 bit) è **l'unità base** nella computazione dell'AES, spesso vengono rappresentati in [[005 Esadecimale e Binario|forma esadecimale]] oppure come polinomio.

$$\{b_7b_6b_5b_4b_3b_2b_1b_0\} \to b_7x^7+b_6x^6+b_5x^5+b_4x^4+b_3x^3+b_2x^2+b_1x^1+b_0$$
Le seguenti operazioni verranno eseguite sulle word da 32 bit (4 byte).
$$w=[a_0,a_1,a_2,a_3]\to a(x)=a_3x^3 + a_2x^2 + a_1x^1 + a_0x^0$$

### Addizione
Somma corrisponde al polinomio i cui coefficienti sono la somma modulo 2 dei coefficienti dei due polinomi. È lo [[029 XOR (AdE)|XOR]] dei byte.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240612110258.png|700]]

### Moltiplicazione
Corrisponde alla moltiplicazione di polinomi modulo un polinomio irriducibile di grado 8. Uno dei 30 polinomi irriducibili per l'AES è $$m(x)=x^8+x^4+x^3+x+1$$
> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240612110845.png|700]]

#### Divisione
È una normale divisione polinomiale, ma viene usato il polinomio irriducibile.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240612111133.png]]

## Descrizione dell'algoritmo
Ogni round consiste in diverse fasi di elaborazione, tra cui una che dipende dalla chiave di crittografia stessa.
- Ottiene l'*input* e lo inserisce in una matrice `state`
- Vengono ricavate le *round keys* tramite l'*espansione della chiave* originale attraverso lo scheduler delle chiavi di AES
- `AddRoundKey`
- 🔁 Per 9, 11 o 13 round:
    1. `SubBytes`
    2. `ShiftRows`
    3. `MixColumns`
    4. `AddRoundKey`
- Ultimo round (per un totale di 10, 12 o 14 round):
	1. `SubBytes` 
	2. `ShiftRows`
	3. `MixColumns`
- Inserisci in una matrice `out` quello che si trova in `state`

### SubBytes
In questa fase, *ogni singolo byte* $S_{r,c}$ della matrice `state` viene sostituito con un byte ottenuto attraverso una [[010 Data Encryption Standard#S-box|S-box]] non lineare. 

$$\textcolor{#61AFEF}{S'_{r,c}=\text{S-box}(S_{r,c})}$$

![[Pasted image 20240612120319.png]]

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240612113849.png|700]]
>
>![[Pasted image 20240612113911.png]]

Per evitare attacchi basati su semplici proprietà algebriche, la S-box viene costruita combinando la funzione inversa con una trasformazione affine invertibile.


### ShiftRows
In questa fase si opera sulle righe di `state`. Viene effettuato uno shift in maniera ciclica di queste righe in posizioni differenti.

![[Pasted image 20240612115419.png|600]]

### MixColumns
In questa fase, ogni word (i quattro bytes di ogni colonna) di `state` sono combinate attraverso una trasformazione lineare invertibile.

![[Pasted image 20240612115615.png|600]]

### AddRoundKey
In questa fase la sottochiave (round key) viene combinata con lo `state`. Per ogni round, la sottochiave è derivata dalla chiave originale attraverso lo scheduler dell'AES.
Ogni byte di `state` viene combinato con un byte della chiave circolare utilizzando uno XOR bit a bit.

![[Pasted image 20240612120005.png|700]]

## Espansione della chiave
A partire dalla chiave originale (matrice `key` di $4\times Nk$ byte), genera le chiavi schedulate (array `w` di $4\times(Nr+1)$ word). 
Se $Nk=4$ (chiave da 128 bit), sono prodotte 44 word.

![[Pasted image 20240612120619.png]]

> [!question]+ Key Derivation Function
> Una Key Derivation Function (KDF) è una funzione crittografica utilizzata per derivare una o più chiavi crittografiche da un input segreto, come una password o una chiave principale:
> - Utilizza un [[031 Autenticazione#^cc0c4f|salt]] per derivare una chiave di cifratura
> - Consente di derivare una chiave di cifratura a partire da una passphrase
> - Consente di derivare una chiave di cifratura utilizzando funzioni hash in maniera iterata

## Decifratura
L'algoritmo di decifratura non è lo stesso della cifratura, infatti usa le *trasformazioni inverse* in una sequenza diversa.

Esiste anche un algoritmo di decifratura che ha la stessa struttura di quello di cifratura:
-  Stessa sequenza di trasformazioni
- Usa le trasformazioni inverse
- Necessita di un cambiamento nella schedulazione della chiave

![[Pasted image 20240612120928.png|800]]

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
- [Wikipedia](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)
- [Mental Outlaw](https://www.youtube.com/watch?v=A8poO23ujxA) oppure [Computerphile](https://www.youtube.com/watch?v=O4xNJsjtN6E)