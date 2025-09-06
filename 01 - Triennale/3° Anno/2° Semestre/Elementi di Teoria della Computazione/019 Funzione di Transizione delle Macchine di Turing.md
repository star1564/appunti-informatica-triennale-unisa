---
aliases:
  - Computazione (Informale) di una Macchina di Turing
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Macchine di Turing
cssclasses:
  - 
---
>Sia $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ una [[018 Macchine di Turing|Macchina di Turing]] deterministica. Il suo **comportamento** è descritto dalla [[004 Automi Finiti Deterministici#Funzione di transizione|Funzione di Transizione]].
>$$\color{#CC241D}\delta:(Q-\{q_{accept},q_{reject}\})\times\Gamma\to Q\times\Gamma\times\{L,R\}$$
>Ovvero, una funzione con: 
>- [[015 Applicazioni#Dominio e Codominio di una Applicazione|dominio]] formato da coppie contenenti:
>	- uno *stato* (tranne quelli finali)
>	- un *simbolo* del nastro 
>- [[015 Applicazioni#Dominio e Codominio di una Applicazione|codominio]] formato da triple contenenti:
>	- uno *stato* (compresi quelli finali)
>	- un *simbolo* del nastro
>	- la *direzione* dello spostamento della testina

> [!warning]+ Indicare lo spostamento a sinistra e a destra
> Per indicare lo spostamento della testina, si scrive una lettera:
> - $\color{#61AFEF}L$ per lo spostamento a sinistra
> - $\color{#61AFEF}R$ per lo spostamento a destra

Supponiamo che:
- $q,q'\in Q$
- $\gamma,\gamma'\in\Gamma$
- $dir\in\{L,R\}$

Se $\delta(q,\gamma)=(q',\gamma',dir)$ e se $M$ si trova nello stato $q$ con la testina posizionata su una cella contenente $\gamma$, alla fine della transizione:
- $M$ si troverà nello stato $q'$
- $\gamma'$ sarà il *simbolo scritto sulla cella* del nastro in cui la testina si trovava all'inizio della transizione (quella che conteneva $\gamma$)
- La testina si sarà *spostata* sulla cella di sinistra se $dir=L$ oppure sulla cella di destra se $dir=R$

> [!note]+ Transizione a sinistra all'inizio del nastro
> Se $M$ tenta di spostare la testina a sinistra quando si trova all'estremità sinistra del nastro, allora la testina **non si sposta**. Resta sulla cella del nastro su cui si trovava all'inizio della transizione anche se la funzione di transizione indica $L$.
> 
> Il cambio di stato avviene comunque.

> [!example]- <font color="orange">Esempio</font>
>![[Pasted image 20240422121641.png|800]]
>
>Se legge il carattere $a$ quando $M$ si trova nello stato $q_0$, allora: 
>1. Scrive $b$ in quella cella
>2. Cambia lo stato da $q_0$ a $q_1$
>3. Sposta la testina a destra di una casella

## Definizione Informale della Computazione di una Macchina di Turing
Inizialmente, una Macchina di Turing $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$:
- Riceve il suo **input** $w=c_1c_2\dots c_n\in\Sigma^*$ sulle $n$ celle più a sinistra del nastro (*parte finita*), mentre il resto del nastro è vuoto (ovvero, contiene *tutti simboli blank*)
- Poiché $\sqcup\not\in\Sigma$, all'inizio della computazione il primo simbolo blank sul nastro individua la **fine dell'input**

![[Pasted image 20240422113656.png|800]]

- La testina si trova sulla posizione più a sinistra del nastro. Tale cella contiene il primo **simbolo di input**.

> [!note]+ Input vuoto
> Se l'input è la stringa vuota $\epsilon$, all'inizio della computazione tutte le celle del nastro contengono il carattere $\sqcup$.

Quando $M$ inizia la computazione, questa procede secondo le regole descritte dalla sua funzione di transizione, cominciando dallo **stato iniziale** $q_0$.

La computazione prosegue fino a quando non viene raggiunto lo stato di *accettazione* oppure quello di *rifiuto*, dove l'input viene accettato o rifiutato. Se nessuno dei due stati viene raggiunto, la computazione di $M$ **continua per sempre**.

> [!note]+ Input non accettato
> $$M\text{ non accetta l'input }w\textcolor{#CC241D}{\centernot\Longleftrightarrow} M\text{ rifiuta l'input }w$$
> $M$ può accettare un input $w$ o non accettare $w$. Se non accetta $w$, questo vuol dire che $M$ *rifiuta* $w$ oppure che la computazione di $M$ su $w$ *non termina*.

^d05899



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