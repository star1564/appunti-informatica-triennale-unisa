---
aliases:
  - Variabile Aleatoria
  - Variabile Stocastica
  - Variabile Randomica
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Variabili Aleatorie
cssclasses:
  - 
---
>Una **Variabile Aleatoria** (*Random Variable*, v.a.) $X$ è una [[011 Funzioni|funzione]] che *associa l'esito di ogni [[009 Esperimento|esperimento]]* dallo [[010 Spazio Campionario|spazio campionario]] *ad un numero* sulla linea dei [[003 Insieme dei numeri Reali|Reali]].
>
>Dato uno [[Spazio di Probabilità]] $(S,\mathcal{F}, \mathbb{P})$, una funzione $X: S\to\mathbb{R}$ è detta variabile aleatoria se risulta
>$$\{X\leq x\}:= \{\omega\in S : X(\omega)\leq x\}\in \mathcal F, \quad \forall x\in\mathbb{R}$$
>![[Pasted image 20240816151914.png|600]]
>Ovvero, che $X$ può avere valore $\leq x\in \mathbb{R}$.

Viene chiamata randomica perché il suo valore dipende dall'output dell'*esperimento casuale*, infatti, il valore è noto solamente dopo che l'esperimento si è concluso.

> [!example]- <font color="orange">Esempio</font>
>Consideriamo ad esempio il risultato del lancio di una moneta. 
>Se la moneta esce testa, assegniamo un valore pari a 1 alla variabile aleatoria $X$, e se esce croce, assegniamo un valore pari a 0 a $X$. 
>
>Pertanto, $X$ è una variabile aleatoria che assume due possibili valori, 0 o 1, a seconda dell'esito del lancio della moneta. 
>
>![[Pasted image 20240228164622.png|700]]

### Individuare gli eventi attraverso le v.a.
Gli [[011 Evento|Eventi]] vengono individuati specificando il valore assegnato agli esiti. Si possono usare i segni $=,\leq, \geq, <, >$.

$$\color{#61AFEF}X=\text{valore esito evento associato}$$
$$\color{#61AFEF}X\lesseqgtr\text{valore esito evento associato}$$

> [!example]- <font color="orange">Esempio</font>
>
>Per specificare l'evento in cui si prendono tutti gli esiti associati a 7, scriviamo $X=7$
>
>![[Pasted image 20240819121130.png|500]]
>Quindi possiamo adesso definirne la probabilità in modo più semplice:
>$$\mathbb{P}(X=7)=\frac{2}{6}=\frac{1}{3}$$
>
>Possiamo anche prelevare eventi con il $\leq$:
>$$\mathbb{P}(X\leq 5)=\frac{4}{6}=\frac{2}{3}$$

### Somma di tutti gli esiti
Dato che la variabile aleatoria può assumere un valore compreso nello spazio campionario, la somma delle probabilità di tutti gli esiti (con probabilità uguale o diversa da tutti gli altri) *è sempre pari a 1*. In pratica, al $100\%$ deve verificarsi uno degli esiti dell'insieme $S$.

> [!example]- <font color="orange">Esempio</font>
>Sia $Y$ il numero di teste che si ottengono nel lancio di 3 monete non truccate. Per $S = \{ccc, cct, ctc, tcc, ctt, tct, ttc, ttt\}$, si ha che $Y : S \rightarrow R$ è una variabile aleatoria che assume i valori $0, 1, 2, 3$ con le seguenti probabilità:
>- $\mathbb{P}(Y = 0) = \mathbb{P}({ccc}) = \frac{1}{8}$
>- $\mathbb{P}(Y = 1) = \mathbb{P}({cct, ctc, tcc}) =\frac{3}{8}$
>- $\mathbb{P}(Y = 2) = \mathbb{P}({ctt, tct, ttc}) = \frac{3}{8}$
>- $\mathbb{P}(Y = 3) = \mathbb{P}({ttt}) = \frac{1}{8}$
>
>![[Pasted image 20230523093950.png]]
>
>Gli eventi $\{Y = k\}, {k=0,1,2,3}$ sono [[011 Evento#^3a631f|necessari]] e [[013 Eventi Compatibili, Incompatibili e Complementari#Eventi Due a Due Incompatibili (o Disgiunti)|Due a Due Incompatibili]], quindi, per via della [[017 Assiomi della Probabilità#Probabilità dell'unione di più eventi incompatibili|proprietà della probabilità dell'unione di più eventi incompatibili]] l'unione delle loro probabilità saranno delle somme.
>
>Pertanto, poiché $Y$ deve assumere uno e uno solo tra i valori $0, 1, 2, 3$ abbiamo che $$\mathbb{P}\Bigg(\bigcup^3_{k=0}\{Y=k\}\Bigg)=\sum_{k=0}^{3} \mathbb{P}(Y=k)=\frac{1}{8}+\frac{3}{8}+\frac{3}{8}+\frac{1}{8}=1$$

### Tipi di variabili aleatorie
Le variabili aleatorie possono essere:
- [[025 Variabili Aleatorie Discrete|Variabili Aleatorie Discrete]]
- [[034 Variabili Aleatorie Continue|Variabili Aleatorie Continue]]


___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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
- [Video (inglese), Statistics 110 - Variabili Aleatorie](https://youtu.be/k2BB0p8byGA?list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&t=499)
- [Andrea Minini](https://www.andreaminini.org/statistica/probabilita/variabili-aleatorie)