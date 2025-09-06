---
aliases: 
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Deterministici
cssclasses:
  - 
---
Una stringa è accettata da un'automa se *esiste un cammino* che parte dallo stato iniziale e termina in uno stato finale. Se questo esiste, allora è anche unico (per via del determinismo).

Questo cammino di accettazione può essere descritto in maniera [[030 Ricorsione di Stringhe|ricorsiva]], infatti *è la [[031 Concatenazione di una Stringa|concatenazione]] del risultato di molte [[004 Automi Finiti Deterministici#Funzione di transizione|Funzioni di Transizione]] $\delta$*.

>Per precisare la nozione di linguaggio di un DFA, è necessario la definizione di una **funzione di transizione estesa $\hat\delta$** (*computazione*), che descrive cosa succede *quando partiamo da uno stato e seguiamo una sequenza* in input.

> [!quote] Definizione Ricorsiva della funzione di transizione estesa [HUM 2.2.4]
> Sia $A=(Q,\Sigma,\delta,q_0,F)$ un automa finito deterministico. La **funzione di transizione estesa $\hat\delta:Q\times\Sigma^*\to Q$** è definita ricorsivamente come segue.
> 
> - **Passo Base**: $\forall q\in Q$
> $$\color{#61AFEF}\hat\delta(q,\epsilon)=q$$
> In altre parole, se ci troviamo nello stato $q$ e non leggiamo alcun input, allora rimaniamo nello stato $q$.
> 
> - **Passo Ricorsivo**: $\forall q\in Q,w\in\Sigma^*,a\in\Sigma$
> $$\color{#61AFEF}\hat\delta(q,wa)=\delta(\hat\delta(q,w),a)$$
> Quindi, per computare $\hat\delta(q,wa)$, calcoliamo prima $\hat\delta(q,w)$, ovvero lo stato in cui si trova l'automa dopo aver elaborato tutti i simboli di $wa$ eccetto l'ultimo ($a$).
> Supponiamo che questo stato sia $p$, ossia $\hat\delta(q,w)=p$, allora $\hat\delta(q,wa)$ è quanto si ottiene compiendo una transizione dallo stato $p$ sull'input $a$, l'ultimo simbolo di $w$, ovvero $\hat\delta(q,wa)=\delta(p,a)$.

Il procedimento sembra essere simile alla [[037 Ricorsione di Alberi|Ricorsione sui nodi degli Alberi]].

Inoltre è *sempre* vero che $\hat\delta(q,a)=\delta(q,a)$.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240310182141.png]]
>$$\Sigma=\{0,1\},\quad \mathcal{L}(M)=\{w\in\Sigma^*\ |\ w\text{ finisce per } 1 \text{ oppure per un numero di volte pari di } 0 \}$$
>Supponiamo di avere $w=0100$. Questa stringa ha un path di accettazione (ovvero finisce su $q_1$)?
>
>$\hat\delta(q_0,0100)= \delta(\overbrace{\hat\delta(q_0, 010)}^\text{STATO}, 0)=$
>
>$= \delta(\delta(\overbrace{\hat\delta(q_0, 01)}^\text{STATO},0), 0)=$
>
>$= \delta(\delta(\delta(\overbrace{\hat\delta(q_0, 0)}^\text{STATO}, 1),0), 0)=$
>
>$= \delta(\delta(\delta(\delta(\hat\delta(q_0, \epsilon), 0), 1),0), 0)=$ [passo base]
>
>$= \delta(\delta(\delta(\delta(q_0, 0), 1),0), 0)=$ 
>
>$= \delta(\delta(\delta(q_0, 1),0), 0)=$ 
>
>$= \delta(\delta(q_1,0), 0)=$ 
>
>$= \delta(q_2, 0)=q_1$ 
>
>Questa stringa ha un path di accettazione.


---

![[005 Linguaggio di un DFA#Definizione Formale del Linguaggio Riconosciuto da un DFA]]


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