---
aliases: 
tags:
  - corsi/informatica/machine_learning
paragrafo: Naive Bayes
cssclasses:
  - 
---
![[022 Formula di Bayes#^014a04]]

---

>Il **Naive Bayes** è un *algoritmo di [[007 Task di Classificazione|classificazione]] probabilistico* basato sul [[022 Formula di Bayes|teorema di Bayes]]. Calcola la *probabilità di ciascuna [[021 Features Engineering#Feature|Feature]] predittiva* dei dati che appartengono a ciascuna classe, per poi fare una predizione della distribuzione di probabilità su tutte le classi.

Naturalmente, dalla distribuzione di probabilità risultante, possiamo concludere la classe più probabile a cui è associato ogni nuovo sample di dati.

Ciò che Naive Bayes fa nello specifico, come indica il suo nome, può essere così descritto:
- **Bayes**: mappa la *probabilità* delle feature di input osservate di una possibile classe alla probabilità della classe sugli input osservati in accordo al teorema di Bayes
- **Naive**: *semplifica il calcolo delle probabilità* assumendo che le feature predittive siano reciprocamente indipendenti.

### Funzionamento
Abbiamo un data sample $x = (x_{1}, x_{2}, \dots, x_{n})$ composto da $n$ feature $x_{1}, x_{2}, \dots, x_{n}$.
L'obbiettivo del Naive Bayes è quello di *determinare le probabilità che questo sample appartenga ad ognuna delle $K$ classi $y_{1}, y_{2}, \dots, x_{K}$*.

Dobbiamo quindi considerare un evento congiunto che consideri i valori delle feature osservate $x = (x_{1}, x_{2}, \dots, x_{n})$ e la probabilità $P(y_K|x)$ che il sample appartenga alla classe $k$.

$$P(y_{K}|x)=\frac{P(x|y_{k})\cdot P(y_{k})}{P(x)}$$

Dove:
- $P(y_k)$ rappresenta *come sono distribuite le classi*, senza ulteriori conoscenze delle caratteristiche dell'osservazione.
	- Viene chiamato **prior** e può essere predeterminato o appreso da un insieme di dati di addestramento.
- $P(x|y_k)$ è detta **likelihood** e rappresenta la *distribuzione congiunta* di $n$ feature considerando che il sample appartenga a $y_{k}$.
	- Sarà difficile da calcolare all'aumentare del numero di feature.
	- Nel Naive Bayes, questo problema viene risolto grazie all'ipotesi di indipendenza delle feature.
		- $\color{#61AFEF}P(x|y_k) = P(x_{1}|y_k)\cdot P(x_{2}|y_k) \cdot \dots \cdot P(x_{n}|y_k)$
- $P(y_k|x)$ è detta **posterior** e ha una conoscenza aggiuntiva *dovuta all'osservazione*.
	- Si calcola moltiplicando il prior per la likelihood.
	- $\color{#61AFEF}P(y_k|x) = P(y_{k})\cdot P(x|y_{k})$
- $P(x)$ è detta **evidenza** e dipende esclusivamente dalla distribuzione complessiva delle feature, quindi rappresenta una *costante di normalizzazione*.
	- Una costante di normalizzazione per evitare che il risultato si riduca a 0 è la tecnica del **Laplace Smoothing**, quando il caso non è osservabile nel training set.


> [!example]- <font color="orange">Esempio</font>
>Dati quattro (pseudo) user, se a ciascuno di loro
>- non piacciono/piacciono tre film $m1$, $m2$, $m3$ (indicati come 0 o 1)
>- non piace/piace un film target (denotato come $N$ o $Y$).
>
>![[Pasted image 20231216165642.png|850]]
>
><i>Qual è la probabilità che ad un altro utente piaccia il film target?</i>
>
>---
>Abbiamo che:
>- $m1,m2,m3$ rappresentano le feature
>- ci sono 4 sample per il training che contengono anche il valore target (Y)
>- dal training set si nota che
>	- $P(Y) = 3/4$
>	- $P(N) = 1/4$
>- denotiamo gli eventi che ad un utente piacciano i tre film con $f_1,f_2,f_3$
>
>Vogliamo calcolare $P(Y|x)$ con $x=(1,1,0)$.
>
>1. Calcoliamo il **Laplace Smoothing** per il denominatore (al numeratore sarà sempre $\textcolor{#FF6611}{+1}$): 
>$$P(\text{valore ricercato}) \cdot \text{\#possibili valori} = 1 \cdot 2 = \color{#FF6611}2$$
>Il valore ricercato sarà Y ed N quindi è sempre 1.
>
>2. Calcoliamo la **likelihood**, sia nel caso del valore target, sia nel caso di un altro valore. Dobbiamo calcolarlo attraverso tutte le feature di $x$.
>
>$$P(f_1 = 1|N)=\frac{P(N|f_1=1)\textcolor{#FF6611}{+1}}{P(N)\textcolor{#FF6611}{+2}} = \frac{0\textcolor{#FF6611}{+1}}{1\textcolor{#FF6611}{+2}} = \color{#CC241D}\frac{1}{3}$$
>
>$0+1 \to$ esistono 0 osservazioni a cui piace il film 1 e il target value è N, poi si aggiunge 1 per lo smoothing
>$1+2 \to$ esiste 1 osservazione (ID=2) con target value uguale a N, poi si aggiunge $1\cdot2 =2$ per lo smoothing, visto che il numero di possibili valori è pari a 2
>
>$$P(f_1 = 1|Y)=\frac{P(Y|f_1=1)\textcolor{#FF6611}{+1}}{P(Y)\textcolor{#FF6611}{+2}} = \frac{1\textcolor{#FF6611}{+1}}{3\textcolor{#FF6611}{+2}} = \color{#61AFEF}\frac{2}{5}$$
>
>$1+1 \to$ esiste 1 osservazione (ID=4) a cui piace il film 1 e il target value è Y, poi si aggiunge 1 per lo smoothing
>$3+2 \to$ esistono 3 osservazioni (ID=1,3,4) con target value uguale a Y, poi si aggiunge $1\cdot2 =2$ per lo smoothing, visto che il numero di possibili valori è pari a 2
>
>
>$$P(f_2 = 1|N)=\frac{P(N|f_2=1)\textcolor{#FF6611}{+1}}{P(N)\textcolor{#FF6611}{+2}} = \frac{0\textcolor{#FF6611}{+1}}{1\textcolor{#FF6611}{+2}} = \color{#CC241D}\frac{1}{3}$$
>
>$0+1 \to$ esistono 0 osservazioni a cui piace il film 2 e il target value è N, poi si aggiunge 1 per lo smoothing
>$1+2 \to$ esiste 1 osservazione (ID=2) con target value uguale a N, poi si aggiunge $1\cdot2 =2$ per lo smoothing, visto che il numero di possibili valori è pari a 2
>
>$$P(f_2 = 1|Y)=\frac{P(Y|f_2=1)\textcolor{#FF6611}{+1}}{P(Y)\textcolor{#FF6611}{+2}} = \frac{2\textcolor{#FF6611}{+1}}{3\textcolor{#FF6611}{+2}} = \color{#61AFEF}\frac{3}{5}$$
>
>$2+1 \to$ esistono 2 osservazioni (ID=1,4) a cui piace il film 2 e il target value è Y, poi si aggiunge 1 per lo smoothing
>$3+2 \to$ esistono 3 osservazioni (ID=1,3,4) con target value uguale a Y, poi si aggiunge $1\cdot2 =2$ per lo smoothing, visto che il numero di possibili valori è pari a 2
>
>$$P(f_3 = 0|N)=\frac{P(N|f_3=0)\textcolor{#FF6611}{+1}}{P(N)\textcolor{#FF6611}{+2}} = \frac{0\textcolor{#FF6611}{+1}}{1\textcolor{#FF6611}{+2}} = \color{#CC241D}\frac{1}{3}$$
>
>$0+1 \to$ esistono 0 osservazioni a cui NON piace il film 3 e il target value è N, poi si aggiunge 1 per lo smoothing
>$1+2 \to$ esiste 1 osservazione (ID=2) con target value uguale a N, poi si aggiunge $1\cdot2 =2$ per lo smoothing, visto che il numero di possibili valori è pari a 2
>
>$$P(f_3 = 0|Y)=\frac{P(Y|f_3=0)\textcolor{#FF6611}{+1}}{P(Y)\textcolor{#FF6611}{+2}} = \frac{2\textcolor{#FF6611}{+1}}{3\textcolor{#FF6611}{+2}} = \color{#61AFEF}\frac{3}{5}$$
>
>$2+1 \to$ esistono 2 osservazioni (ID=3,4) a cui NON piace il film 3 e il target value è Y, poi si aggiunge 1 per lo smoothing
>$3+2 \to$ esistono 3 osservazioni (ID=1,3,4) con target value uguale a Y, poi si aggiunge $1\cdot2 =2$ per lo smoothing, visto che il numero di possibili valori è pari a 2
>
>$$\text{Likelihood}(N) = \textcolor{#CC241D}{\frac{1}{3}\cdot\frac{1}{3}\cdot\frac{1}{3}}=0,037$$
>
>$$\text{Likelihood}(Y) = \textcolor{#61AFEF}{\frac{2}{5}\cdot\frac{3}{5}\cdot\frac{3}{5}}=0,144$$
>
>3. Calcoliamo i **Posterior**:
>$$Posterior(N)=P(N)\cdot\text{Likelihood}(N) =\frac{1}{4}\cdot 0,037 = 0,009$$
>$$Posterior(Y)=P(Y)\cdot\text{Likelihood}(Y) =\frac{3}{4}\cdot 0,144 = 0,108$$
>
>4. Normalizziamo attraverso la [[019 Gestire Dataset Sbilanciati#^348afa|Precision]]:
>
>$$P(N|x) = \frac{0,009}{0,009+0,108} = 0,078$$
>$$P(Y|x) = \frac{0,108}{0,108+0,009} = 0,922$$
>
>Abbiamo il 92,2% di probabilità che al nuovo utente piaccia il film target.


___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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