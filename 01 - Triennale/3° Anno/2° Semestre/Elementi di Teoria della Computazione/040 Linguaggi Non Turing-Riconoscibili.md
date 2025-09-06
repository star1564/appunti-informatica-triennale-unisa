---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Indecidibilità
cssclasses:
  - 
---
>Un linguaggio $L$ **Non è [[023 Linguaggio Turing-Riconoscibile|Turing-Riconoscibile]]** se non esiste alcuna [[018 Macchine di Turing|Macchina di Turing]] $M$ capace di riconoscere $L$. 
>
>Ovvero che qualsiasi macchina *non riesce ad identificare* tutte le stringhe che appartengono o non al linguaggio.

^89609e

## Dimostrazione

È possibile usare il [[038 Metodo della Diagonalizzazione#Metodo della diagonalizzazione per la Cardinalità|Metodo della Diagonalizzazione]] per provare che *esistono linguaggi che non sono [[023 Linguaggio Turing-Riconoscibile|Turing-Riconoscibili]]*.

La dimostrazione consiste nel provare le seguenti affermazioni:
1. Dato un alfabeto $\Sigma$, l'insieme $\Sigma^*$ *è [[039 Numerabilità#^4f7a3f|numerabile]]*
2. L'insieme delle [[036 Codifiche|codifiche]] delle Macchine di Turing (quindi l'insieme delle Macchine di Turing) *è numerabile*
3. L'insieme dei linguaggi Turing-Riconoscibili *è numerabile*, quindi ad ogni linguaggio Turing-Riconoscibile è associata la codifica di una Macchina di Turing
4. L'insieme dei linguaggi sull'alfabeto $\Sigma$ ha [[038 Metodo della Diagonalizzazione#^8e6455|cardinalità maggiore]] del numerabile (*non è numerabile*)

> [!warning] Questa prova non è costruttiva


---
### 1. $\quad$Numerabilità dell'insieme delle parole

![[039 Numerabilità#^006c31]]

>L'insieme **$\Sigma^*$ è numerabile**. 

Lo si dimostra attraverso l'unione di insiemi numerabili $\Sigma^*=\bigcup_{n\in\mathbb{N}}\Sigma^n$.

Sia $\Sigma=\{a_1,a_2,\dots,a_k\}$ un alfabeto. Possiamo definire una corrispondenza [[021 Applicazioni Biettive|biunivoca]] tra $\mathbb{N}$ e $\Sigma^*$ che permette di [[039 Numerabilità#^a70d8d|enumerare]] le stringhe in [[028 Ordine Radix|ordine radix]]: $$w_1=\epsilon,w_2=a_1,w_3=a_2,\dots$$

> [!example]- <font color="orange">Esempio</font>
> Se $\Sigma=\{a,b\}$, la sequenza radix è $$\epsilon,a,b,aa,ab,ba,bb,\dots$$
> Quindi $$w_1=\epsilon,w_2=a,\dots,w_6=ba,\dots$$

In generale, per la stringa $w_n$ abbiamo che $$\color{#CC241D}n=|\{y\in\Sigma^*:y<w_n\text{ (rispetto all'ordine radix)}\}| + 1$$

È possibile definire un algoritmo per tale indice. Quindi abbiamo una funzione *biettiva e calcolabile* di $\mathbb{N}$ in $\Sigma^*$.

---
### 2. $\quad$Numerabilità dell'insieme delle codifiche delle Macchine di Turing
> L'insieme delle **codifiche delle Macchine di Turing è numerabile**. Questo è $$C=\{\langle M \rangle\ |\ M \text{ è una Macchina di Turing}\}$$

È possibile [[036 Codifiche#Codifica di una Macchina di Turing|codificare una Macchina di Turing]] $M$ con una stringa su un alfabeto $\Sigma$ e l'applicazione $f:\langle M \rangle\to \langle M \rangle\in\Sigma^*$ è [[020 Applicazioni Iniettive|iniettiva]]. 
Quindi: $$|C| \leq |\Sigma^*| = |\mathbb{N}|$$
Sia $w$ una stringa su un alfabeto $\Delta$. Il linguaggio $\{w\}$ è [[024 Macchine Decisionali#Linguaggio Decidibile|decidibile]], sia $M_w$ una fissata Macchina di Turing che decide $\{w\}$. L'applicazione $g$ tale che $g(w)=\langle M_w \rangle$ è iniettiva. 

Quindi: $$|\mathbb{N}|=|\Delta^*|\le |C|$$
Da cui: $$\color{#CC241D}|C|=|\mathbb{N}|$$

---
### 3. $\quad$Numerabilità dell'insieme dei linguaggi Turing-Riconoscibili
> L'insieme dei **linguaggi Turing-Riconoscibili è numerabile**. Questo è $$R=\{L\subseteq\Sigma^*\ |\ L \text{ è un linguaggio Turing-Riconoscibile}\}$$

Possiamo associare ad ogni linguaggio Turing-Riconoscibile una fissata Macchina di Turing che lo riconosce. Questa corrispondenza è iniettiva, quindi: $$|R|\le|C|=|\mathbb{N}|$$
Dove $C$ è l'insieme delle codifiche delle Macchine di Turing.

Sia $w$ una stringa su un alfabeto $\Delta$. Il linguaggio $\{w\}$ è Turing-Riconoscibile. L'applicazione $h$ tale che $h(w)=\{w\}$ è iniettiva. 

Quindi: $$|\mathbb{N}| =|\Delta^*|\le|R|$$

Da cui: $$\color{#CC241D}|R|=|\mathbb{N}|$$

---
### 4. $\quad$Non Numerabilità dell'insieme dei Linguaggi

>L'insieme dei linguaggi $\color{#CC241D}\mathcal{P}(\Sigma^*)$ **su $\Sigma$ non è numerabile**.

> [!note]+ Sequenza Binaria Infinita
> È una [[026 Sequenza|sequenza]] di 0 e 1 che non termina.

Siano $\Sigma^*=\{w_1,w_2,\dots\}$ e $\mathcal{B}$ l'insieme delle sequenze binarie infinite.

Ad un qualsiasi linguaggio $L$ può essere associata una sequenza binaria infinita $\mathcal{X}_L$, chiamata **Sequenza Caratteristica** del linguaggio $L$, dove il bit $i$-esimo è:
- $1$ se l'$i$-esima stringa $w_i\in L$
- $0$ se l'$i$-esima stringa $w_i\not\in L$

> [!example]+ <font color="orange">Esempio</font>
>La sequenza binaria del linguaggio delle stringhe binarie che iniziano con $0$:
>
>![[Pasted image 20240513123733.png]]

Questa relazione è una **corrispondenza biunivoca** tra l'insieme $\mathcal{B}$ e $\mathcal{P}(\Sigma^*)$ dei linguaggi su $\Sigma$, ovvero:
- Per ogni linguaggio $L\in\mathcal{P}(\Sigma^*)$ su $\Sigma$ è associata *una sola sequenza binaria infinita* $\color{#61AFEF}\mathcal{X}_L$
- Per ogni sequenza binaria infinita $\mathcal{X}_L$ è associato *un solo linguaggio* $\color{#61AFEF}L\in\mathcal{P}(\Sigma^*)$ *su $\Sigma$*

Quindi, l'applicazione $f:\mathcal{P}(\Sigma^*)\to\mathcal{B}$ definita da $f(L)=\mathcal{X}_L$ è biettiva, quindi $|\mathcal{B}| = |\mathcal{P}(\Sigma^*)|$.
Poiché, essendo *$\mathcal{B}$ non numerabile* (dimostrato sotto), anche $\color{#61AFEF}\mathcal{P}(\Sigma^*)$ *non è numerabile*.

#### Non Numerabilità dell'insieme delle sequenze binarie

>L'insieme $\mathcal{B}$ delle sequenze binarie infinite **non è numerabile**.

Per dimostrare questo, si utilizza il metodo della diagonalizzazione in modo simile [[039 Numerabilità#Non Numerabilità|a quello per l'insieme dei reali]].

Mostriamo che non esiste alcuna funzione biettiva di $\mathbb{N}$ sull'insieme $\mathcal{B}$ delle sequenze binarie infinite. Supponiamo per assurdo che esista una funzione biettiva $f$ di $\mathbb{N}$ su $\mathcal{B}$.

Allora possiamo costruire una tabella dove, in generale $d_{i,1}d_{i,2}\dots$ è la sequenza binaria infinita associata ad $f(i)$.

![[Pasted image 20240511153058.png]]

Le cifre sulla diagonale di questa matrice $d_{1,1}d_{2,2}\dots$ definiscono una sequenza binaria infinita $\mathcal{X}$.

La sequenza binaria infinita $\overline{\mathcal{X}}=\overline{d}_{1,1}\overline{d}_{2,2},\dots$, che si ottiene scegliendo in ogni posizione il complemento della corrispondente cifra in $\mathcal{X}$, *non è immagine di nessun intero positivo*.

Ovvero che per ogni $i$, si ha che $\overline{\mathcal{X}}\neq f(i)$ perché $\overline{d}_{i,i}\neq d_{i,i}$. Quindi $\mathcal{B}$ *non è numerabile*.

---
### Conclusione
L'insieme $R=\{L\subseteq\Sigma^*\ |\ L \text{ è un linguaggio Turing-Riconoscibile}\}$ è numerabile, mentre $\mathcal{P}(\Sigma^*)$ non è numerabile. Quindi $$|R| = \color{#61AFEF}|\mathbb{N}|\ne|\mathcal{P}(\Sigma^*)|$$

Inoltre la funzione $h:R\to\mathcal{P}(\Sigma^*)$, dove $h(L)=L$, è *iniettiva*. Ne consegue che $$|R| = \color{#CC241D}|\mathbb{N}| < |\mathcal{P}(\Sigma^*)|$$

Quindi *esistono linguaggi che non sono Turing-Riconoscibili*.





___
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
- [[Terminologie Linguaggi MdT.canvas|Canvas dei linguaggi delle MdT]]