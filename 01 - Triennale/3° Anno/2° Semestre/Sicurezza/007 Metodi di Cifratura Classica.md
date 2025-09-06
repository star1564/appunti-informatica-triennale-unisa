---
aliases:
  - Cifrario di Cesare
  - Cifrario a sostituzione monoalfabetica
  - Cifrario di Playfair
  - Cifrario di Vigenère
  - One-Time Pad
tags:
  - corsi/informatica/sicurezza
paragrafo: Crittografia
cssclasses:
  - 
---
Queste tecniche classiche sono di tipo [[004 Cifrari Simmetrici|simmetrico]].

## Tecniche di Trasposizione (Basate su matrici)
Scrivere il messaggio in una matrice, riga per riga. Scrivere la chiave su ogni colonna della matrice.

Per criptare, si riorganizzano le colonne in base all'ordine del numero della colonna e si ottiene il messaggio criptato, leggendo colonna per colonna.

Per decifrare: 
- rimettere tutte le lettere come se si volesse creare un nuovo messaggio
- osservare nell'ultima colonna gli spazi pieni e vuoti
- creare una nuova tabella e inserire in base all'ordine del numero di colonna le lettere.

> [!example]- <font color="orange">Esempio</font>
>#### Criptare
>Messaggio: "MESSAGGIOSEGRETO1"
>
>| *4* | *1* | *3* | *2* | *5* |
>| --- | --- | --- | --- | --- |
>| M   | E   | S   | S   | A   |
>| G   | G   | I   | O   | S   |
>| E   | G   | R   | E   | T   |
>| O   | 1   |     |     |     |
>
>
>
>| *1* | *2* | *3* | *4* | *5* |
>| --- | --- | --- | --- | --- |
>| E   | S   | S   | M   | A   |
>| G   | O   | I   | G   | S   |
>| G   | E   | R   | E   | T   |
>| 1    |     |     | O   |     |
>
>Messaggio cifrato: "EGG1SOESIRMGEOAST"
>
>#### Decriptare
>| *4* | *1* | *3* | *2* | *5* |
>| --- | --- | --- | --- | --- |
>| E   | G   | G   | S   | O   |
>| E   | S   | I   | R   | M   |
>| G   | E   | O   | A   | S   |
>| <font color="#CC241D">T</font>   | <font color="#CC241D">1</font>   |     |     |     |
>
>
>Da notare che nella colonna 4 e 1 ci devono essere quattro lettere.
>
>Creare una nuova tabella ed inserire nelle colonne le lettere, tenendo presente se in quella colonna bisogna inserire un carattere in più.
>
>| *1* | *2* | *3* | *4* | *5* |
>| --- | --- | --- | --- | --- |
>| E   | S   | S   | M   | A   |
>| G   | O   | I   | G   | S   |
>| G   | E   | R   | E   | T   |
>| 1   |     |     | O   |     |
>
>Riorganizzare le colonne in base alla chiave fornita
>
>
>| *4* | *1* | *3* | *2* | *5* |
>| --- | --- | --- | --- | --- |
>| M   | E   | S   | S   | A   |
>| G   | G   | I   | O   | S   |
>| E   | G   | R   | E   | T   |
>| O   | 1   |     |     |     |
>
>Messaggio decifrato: "MESSAGGIOSEGRETO1"

## Tecniche di Sostituzione
### Cifrario di Cesare
Il **Cifrario di Cesare** prevede la sostituzione di ogni lettera dell'alfabeto con la lettera *che si trova tre posizioni più in basso* nell'alfabeto.
Si noti che l'alfabeto è avvolto su se stesso, in modo che la lettera che segue la Z sia la A.

![[Pasted image 20240313173959.png]]

Sostituendo le lettere con un numero equivalente abbiamo:

![[Pasted image 20240313173854.png]]

Quindi il carattere criptato sarà dato da: $$encrypt(c)= (c+3)\ mod\ 26$$

Mentre con un numero tra 1 e 25 deciso a priori: $$encrypt(c,k)=(c+k)\ mod\ 26$$
L'algoritmo di decifrazione sarà: $$decrypt(c,k)=(c-k)\ mod\ 26$$

> [!error]+ Troppo semplice da rompere
> Se si sa che un dato testo cifrato è un Cifrario di Cesare, è facile eseguire una [[003 Crittografia#^20ecc3|Crittoanalisi]] a [[003 Crittografia#^9a60da|Brute Force]]: basta provare tutte le 25 chiavi possibili.

### Cifrario a sostituzione monoalfabetica
Un **cifrario a sostituzione monoalfabetica** è un sistema crittografico che utilizza un alfabeto per il testo in chiaro e una [[003 Permutazioni|permutazione]] dello stesso per il testo cifrato. La permutazione utilizzata costituisce la *chiave* del sistema. Durante la cifratura, ad ogni lettera del testo in chiaro viene associata la corrispondente lettera dell'alfabeto permutato.

Questo permette di avere $26!$ chiavi.

![[Pasted image 20240313175146.png|700]]

> [!error]+ Suscettibile alla crittoanalisi
> Se il crittoanalista conosce la natura del testo in chiaro (ad esempio, un testo inglese non compresso), può sfruttare le regolarità del linguaggio.
> 
> ![[Pasted image 20240313175410.png|500]]

La crittoanalisi può essere resa più complessa con l'inserimento di:
- *Nulle* - Simboli meno frequenti in posizioni che non alterano il significato (es. `QUELQRAMODELQLAGO`)
- *Omofoni* - Molti simboli scelti a caso per cifrare singoli caratteri frequenti

### Cifrario Playfair
Il più noto cifrario a lettere multiple è il **Playfair**, che tratta i digrammi del testo in chiaro come singole unità e li traduce in digrammi del testo cifrato.
L'algoritmo Playfair si basa sull'uso di una *matrice di lettere $5\times 5$* costruita utilizzando una parola chiave.

![[Pasted image 20240502100320.png]]
In questo caso, la parola chiave è `MONARCHY`.

La matrice si costruisce riempiendo le lettere della parola chiave (meno i duplicati) da sinistra a destra e dall'alto in basso, e poi riempiendo il resto della matrice con le *lettere rimanenti in ordine alfabetico*. Le lettere I e J contano come una lettera.

Il testo in chiaro viene crittografato **due lettere alla volta**, secondo le seguenti regole:
1. Le *lettere ripetute* del testo in chiaro che si trovano nella stessa coppia sono separate da una *lettera filler*, come la `x`, in modo che la parola `balloon` venga trattata come `ba lx lo on`.
2. Due lettere del testo in chiaro che cadono *nella stessa riga della matrice* vengono entrambe sostituite dalle *lettere a destra*, con il primo elemento della riga che segue circolarmente l'ultimo. Ad esempio, `ar` viene cifrato come `RM`.
3. Due lettere del testo in chiaro che cadono *nella stessa colonna* vengono sostituite entrambe dalla *lettera sottostante*, con il primo elemento della colonna che segue circolarmente l'ultimo. Ad esempio, `mu` viene cifrato come `CM`.
4. Altrimenti, ogni lettera del testo in chiaro di una coppia viene sostituita dalla lettera che si trova *nella sua stessa riga* e *nella colonna occupata dall'altra lettera* del testo in chiaro. In questo modo, `hs` diventa `BP`, mentre `ea` diventa `IM` (o `JM`).

> [!error]+ Suscettibile alla crittoanalisi
>Anche con $26\times26=676$ diagrammi, il cifrario Playfair è relativamente facile da decifrare, perché lascia intatta gran parte della struttura del linguaggio del testo in chiaro. In genere sono sufficienti poche centinaia di lettere di testo cifrato.

### Cifrario di Hill
Questo algoritmo di crittografia prende $m$ lettere successive del testo in chiaro e le *sostituisce con $m$ lettere del testo cifrato*. La sostituzione è determinata da $m$ *equazioni lineari* in cui a ogni carattere è assegnato un valore numerico $(a = 0, b = 1, \dots, z = 25)$.

Per $m=3$, la cifratura delle tre lettere $p_1p_2p_3$ è
$$c_1=(k_{11}p_1 + k_{12}p_2 + k_{13}p_3)\mod{26}$$
$$c_2=(k_{21}p_1 + k_{22}p_2 + k_{23}p_3)\mod{26}$$
$$c_3=(k_{31}p_1 + k_{32}p_2 + k_{33}p_3)\mod{26}$$

Ovvero la matrice 

$$
\begin{pmatrix} c_1\\ c_2\\ c_3 \end{pmatrix}=
\begin{pmatrix} k_{11} & k_{12} & k_{13}\\ k_{21} & k_{22} & k_{23}\\ k_{31} & k_{32} & k_{33} \end{pmatrix} \times
\begin{pmatrix} p_1\\ p_2\\ p_3 \end{pmatrix} \mod{26}
$$

> [!example]- <font color="orange">Esempio</font>
>Consideriamo il testo in chiaro `paymoremoney` e utilizzare la seguente chiave
>$$K= \begin{pmatrix} 17 & 17 & 5\\ 21 & 18 & 21\\ 2 & 2 & 19 \end{pmatrix}$$
>
>Le prime tre lettere del testo in chiaro sono rappresentate dal vettore $(15\ 0\ 24)$. Quindi 
>$$
\begin{pmatrix} c_1\\ c_2\\ c_3 \end{pmatrix}=
\begin{pmatrix} 17 & 17 & 5\\ 21 & 18 & 21\\ 2 & 2 & 19 \end{pmatrix} \times
\begin{pmatrix} 15\\ 0\\ 24 \end{pmatrix} \mod{26} =
\begin{pmatrix} 17\\ 17\\ 11 \end{pmatrix} = RRL
>$$
>Continuando così, il testo cifrato per l'intero testo in chiaro è `RRLMWBKASPDH`.

Per la decifratura si usa la matrice inversa, quindi $K$ deve essere invertibile.

### Cifrario di Vigenère
Questo cifrario è di tipo *sostituzione polialfabetica* ed è simile a quello di [[#Cifrario di Cesare|Cesare]], ma più sicuro. 

Possiamo esprimere il **cifrario di Vigenère** nel modo seguente. 

Si supponga di avere una sequenza di lettere in chiaro $P = p_0, p_1, p_2, \dots, p_{n-1}$ e una *chiave* costituita dalla sequenza di lettere $K = k_0, k_1, k_2, \dots, k_{m-1}$, dove tipicamente $m < n$. 

La sequenza di lettere cifrate $C = C_0, C_1, C_2, \dots, C_{n-1}$ è calcolata come segue:
$$C_i=p_i+k_{i\text{ mod }m}\text{ mod }26$$
Per decifrare il messaggio si calcola:
$$p_i=C_i-k_{i\text{ mod }m}\text{ mod }26$$

> [!example]- <font color="orange">Esempio</font>
>Testo in chiaro: `CODICE MOLTO SICURO`
>Chiave: `REBUS`
>
>`CODIC EMOLT OSICU RO`  ← testo in chiaro
>`REBUS REBUS REBUS RE`  ← chiave
>
>`TSECU VQPFL FWJWM IS`  ← testo cifrato

> [!question] [Enigma](https://it.wikipedia.org/wiki/Enigma_(crittografia))
> Usava tre rotori ed un disco per l'involuzione.


### Cifrario di Alberti
Usa *più alfabeti cifranti* e li *sostituisce* durante la cifratura. Questo attraverso due dischi:
- uno esterno fisso
- uno interno mobile

![[Pasted image 20240617143620.png]]

### One-Time Pad
Lo schema **One-Time Pad** è *impossibile da rompere*. Utilizza una *chiave casuale* lunga quanto il messaggio, in modo da non doverla ripetere. Inoltre, la chiave deve essere utilizzata per cifrare e decifrare un solo messaggio e poi *viene scartata*. Ogni nuovo messaggio richiede una nuova chiave della stessa lunghezza del nuovo messaggio.

Produce un *output casuale* che non ha alcuna relazione statistica con il testo in chiaro. Poiché il testo cifrato non contiene alcuna informazione sul testo in chiaro, non c'è modo di decifrare il codice.

Si supponga di avere una sequenza di lettere in chiaro $P = p_0, p_1, p_2, \dots, p_{n-1}$ e una chiave costituita dalla sequenza di lettere $K = k_0, k_1, k_2, \dots, k_{m-1}$, dove tipicamente $m < n$.

La sequenza di lettere cifrate $C = C_0, C_1, C_2, \dots, C_{n-1}$ è calcolata attraverso lo [[029 XOR (AdE)|XOR]] dei bit:
$$C_i=p_i\oplus k_i$$



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