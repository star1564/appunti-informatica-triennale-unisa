---
aliases: 
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Aritmetica
cssclasses:
  - 
---
#### ADDIZIONE
Se $a, b \in \mathbb{N_0}$ è definita, allora la loro somma *$a+b \in \mathbb{N_0}$*.
L'operazione di *Addizione* gode di alcune proprietà:
- Proprietà **Commutativa**: $a+b = b+a,\quad$con $\forall a, b\in \mathbb{N_0}$; ^cbb1f9
- Proprietà **Associativa**: $(a+b)+c = a+(b+c),\quad$con $\forall a, b, c\in \mathbb{N_0}$; ^6107be
- L'**Elemento Neutro** rispetto all'addizione è 0: $a+1 = a = 0 + a,\quad$con $\forall a\in \mathbb{N_0}$;
- $a + b = 0 \implies a=b=0$;
- **Cancellabilità** rispetto alla addizione: $a+c = b+c \implies a = b,\quad$con $\forall a, b, c\in \mathbb{N_0}$.

#### MOLTIPLICAZIONE
Se $a, b \in \mathbb{N_0}$ è definita, allora il loro prodotto *$a\times b \in \mathbb{N_0}$*.
In seguito si ci riferirà alla moltiplicazione in questo modo: *$ab \in \mathbb{N_0}$*.
L'operazione di *Moltiplicazione* gode di alcune proprietà:
- Proprietà **Commutativa**: $a\times b = b\times a,\quad$con $\forall a, b\in \mathbb{N_0}$;
- Proprietà **Associativa**: $(a\times b)\times c = a\times (b\times c),\quad$con $\forall a, b, c\in \mathbb{N_0}$;
- L'**Elemento Neutro** rispetto alla moltiplicazione è 1: $a\times 1 = 1 = 1 \times a,\quad$con $\forall a\in \mathbb{N_0}$;
- $a\times 0 = 0 = 0 \times a,\quad$con $\forall a\in \mathbb{N_0}$;
- **Legge di annullamento del prodotto**: $a\times b = 0 \implies a=0 \lor b=0$
- $a\times b = 1 \implies a = b = 1$
- **Cancellabilità** rispetto al prodotto di numeri positivi: $a\times c = b\times c \land c \neq 0\implies a = b,\quad$con $\forall a, b\in \mathbb{N_0},\quad c\in \mathbb{N}$;
- Proprietà **Distributiva** della moltiplicazione rispetto all'addizione: $a\times (b+c) = a\times b + a \times c,\quad$con $\forall a, b, c\in \mathbb{N_0}$.

#### POTENZE
Si definiscono *Potenze* dei numeri naturali:
$a^n=a\times a\times a\times...\times a,\quad$con $\forall a, n\in \mathbb{N_0}$, dove a viene ripetuto n volte.
Le potenze così definite godono di alcune proprietà ($\forall a,b,n,m\in \mathbb{N_0}$): ^08910a
- Per convenzione **$a^0 = 1$**;
- **$a^n\times a^m = a^{n+m}$**; ^e03c15
- **$(a^n)^m=a^{nm}$**;
- **$(a\times b)^n=a^nb^n$**.

#### DIVISIONE
Se $a,b \in \mathbb{N_0}$ è definita, si dice che *$a$ divide $b$* oppure *$a$ è un divisore di $b$*, quando esiste un numero $k \in \mathbb{N_0}$ tale che $b$ si possa raggiungere moltiplicando $a$ per $k$, ovvero:
$$a|b \iff \exists k \in \mathbb{N_0}: b =  ak$$
L'operazione di *Divisione* gode di alcune proprietà:
- Proprietà **Riflessiva**: $a|a,\quad$ con $\forall a\in \mathbb{N_0}\quad$ è sempre vera perché $a=a\times 1$;
- **$a|0,\quad$** con $\forall a\in \mathbb{N_0}\quad$ è sempre vera perché $0=a\times 0$;
- **$1|a,\quad$** con $\forall a\in \mathbb{N_0}\quad$ è sempre vera perché $a=1\times a$;
- Proprietà **Asimmetrica**: se $a|b$ e $b|a \implies a=b,\quad$con $\forall a,b\in \mathbb{N_0}$. Perché:

![[Pasted image 20211023150542.png|660]]
- Proprietà **Transitiva**: se $a|b$ e $b|c \implies a|c,\quad$con $\forall a,b,c\in \mathbb{N_0}$.
- Se $a|b$ e $a|c \implies a|b+c,\quad$con $\forall a,b,c\in \mathbb{N_0}$. Perché:

![[Pasted image 20211023150821.png|450]]
___
[[000 Indice MD|↖ Ritorna all'indice ↖]]

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