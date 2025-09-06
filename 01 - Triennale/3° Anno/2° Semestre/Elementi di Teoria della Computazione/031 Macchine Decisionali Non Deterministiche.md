---
aliases:
    - Linguaggi Decidibili delle MdT non deterministiche
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Varianti delle Macchine di Turing
cssclasses:
    - 
---

> [!tip]- Decisore
> ![[024 Macchine Decisionali#^63f682]]

---

> Una [[029 Macchina di Turing Non Deterministica|Macchina di Turing Non Deterministica]] $N$ **decide** $L$ se:
>
> 1.  $N$ è un decisore
> 2.  $L=\mathcal{L}(N)$

Ovvero è una macchina non deterministica se tutte le ramificazioni si fermano su ogni input.

> [!note]- Nota
> Sia $N$ una Macchina di Turing Non Deterministica tale che, per ogni input $w$, $N$ accetta $w$ o $N$ rifiuta $w$.
>
> $N$ non è necessariamente un decisore.
>
> ![[Pasted image 20240506162202.png|500]]


## Linguaggi Decidibili di Macchine di Turing Non Deterministiche

> Un linguaggio $L$ è [[024 Macchine Decisionali#Linguaggio Decidibile|decidibile]] se e solo se esiste una Macchina di Turing Non Deterministica $N$ che lo decide.

> [!quote]+ Prova
> Se un linguaggio $L$ è decidibile, esiste una macchina di Turing Deterministica $M$ che è un [[024 Macchine Decisionali|decisore]] e che accetta $L$.
> $M$ può essere facilmente [[030 Equivalenza Macchina di Turing Deterministica e Non|trasformata]] in una Macchina di Turing Non Deterministica che decide $L$.
>
> Viceversa, se un linguaggio $L$ è deciso da una macchina di Turing Non Deterministica $N$, definiamo una macchina di Turing deterministica $D'$, modificando la precedente definizione di $D$ come segue:
>
> 1.  Inizialmente il nastro 1 contiene l'input $\color{#61AFEF}w$ ed i nastri 2 e 3 sono _vuoti_ (contengono _solo $\sqcup$_). ^826204
> 2.  $D'$ _copia il nastro 1 sul nastro 2_ ed inizializza la stringa sul nastro 3 ad $\color{#61AFEF}\epsilon$. ^5a9818
> 3.  Utilizza il nastro 2 per simulare $N$ con input $w$ _su una ramificazione_ della sua computazione non deterministica corrispondente alla stringa sul nastro 3, cioè _riproduce la successione di scelte_ del corrispondente cammino (se tale stringa corrisponde a una computazione).
>     Prima di ogni passo di $N$, consulta il simbolo successivo sul nastro 3 per determinare quale scelta fare tra quelle consentite dalla funzione di transizione di $N$. ^832900
>     -   Se incontra una _configurazione di accettazione_, allora _accetta l'input_. Pertanto, $D'$ accetta $w$ se e solo se $N$ accetta $w$, quindi $\color{#CC241D}\mathcal{L}(D')=\mathcal{L}(N)$.
>     -   Altrimenti, se _non rimangono più simboli_ sul nastro 3 o se questa scelta non deterministica _non è valida_ o se si incontra una _configurazione di rifiuto_, interrompe questo cammino andando alla fase 4.
> 4.  <font color="#e86162">Rifiuta se tutti i cammini dell'albero delle computazioni di $N$ su $w$ hanno portato a una configurazione di rifiuto. Altrimenti $D'$ passa alla fase 5.</font>
> 5.  $D'$ genera sul nastro 3 la stringa successiva a quella corrente in [[028 Ordine Radix|ordine radix]] e torna alla fase 2.
>
> Possiamo dedurre che $D'$ è un decisore per $L$. Se $N$ accetta il suo input $w$, allora $D'$ troverà un cammino che termina in una configurazione di accettazione e $D'$ accetta $w$. Gli altri cammini in $N$ terminano in una configurazione di rifiuto perché $N$ è un decisore.
>
> Se $N$ rifiuta il suo input $w$, tutte le sue computazioni su $w$ terminano in una configurazione di rifiuto. Siccome $N$ è un decisore, ciascuno dei cammini ha un numero finito di nodi poiché ogni arco nel cammino rappresenta un passo di una computazione di $N$ su $w$.
>
> Ovviamente nell'albero di computazione di $N$ su $w$, ogni nodo ha un numero finito di figli. Pertanto l'intero albero di computazione di $N$ su $w$ è finito. Quindi, $D'$ si fermerà e rifiuterà quando l'intero albero sarà stato esplorato.

Nota che la macchina $D'$ deve individuare se tutte le computazioni su $w$ conducono a una configurazione di rifiuto.
Quindi $D'$ deve avere un controllo sulle stringhe che rappresentano codifiche di computazioni su $w$ che terminano in $q_{reject}$ .

Se $x$ è una stringa che codifica una tale computazione, $D'$ deve bloccare la generazione di stringhe in ordine radix con prefisso $x$.
Analogamente, se la stringa $x$ è una stringa non valida, cioè non corrisponde a una computazione, $D'$ deve bloccare la generazione di stringhe in ordine radix con prefisso $x$.

---

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
