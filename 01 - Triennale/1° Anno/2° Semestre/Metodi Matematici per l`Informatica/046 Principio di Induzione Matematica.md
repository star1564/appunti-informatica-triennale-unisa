---
aliases: 
tags:
  - corsi/matematica/metodi_matematici_informatica
paragrafo: Dimostrazioni per induzione
cssclasses:
  - 
---
Molte proprietà matematiche sono vere per ogni intero positivo $n$, questo lo si può dimostrare con il **Principio di Induzione Matematica**.

> [!warning] Numeri molto grandi
> Possiamo provare che un enunciato è vero per certi numeri del dominio, andando a sostituirli nella formula.
> Però con questo metodo non si è data una dimostrazione che la formula sia vera per *tutti* i numeri del dominio, dato che il *controesempio* (una prova per confutare la verità dell'enunciato su un intero dominio) potrebbe essere un numero molto grande. 

Il principio di induzione permette di *dimostrare una proposizione o un teorema* in un insieme composto da infiniti elementi nell'insieme dei numeri naturali, senza doverlo dimostrare per ogni singolo elemento.

> [!question]- Perché il Principio di Induzione Matematica è una tecnica corretta di prova?
> Perché la prova è basata sull'[[001 Definizioni Varie#Assioma (Postulato)|assioma]] che $\mathbb{N}$ è un [[031 Tipi di Insiemi di Relazioni#Insieme Ben Ordinato|insieme ben ordinato]].

## Procedimento
Per provare che una funzione proposizionale $P(n)$ è vera *per ogni numero intero positivo $\color{#61AFEF}n$* ($\mathbb{N_0}-\{0\}$), basta provare i seguenti due enunciati.

>**BASE**: $P(1)$ è vera.
>
>**PASSO INDUTTIVO**: L'[[006 Implicazione|implicazione]] $P(k)\Rightarrow P(k+1)$ è vera per ogni intero positivo $k$. 
>Quindi se $P(k)$ è vero (*Ipotesi Induttiva*), allora $P(k+1)$ è vero.

---
> [!summary]+ Schema da seguire
> In generale, quando si vuole utilizzare il principio di induzione, si usa il seguente schema.
> 
> 1. Esprimi l'enunciato da provare nella forma "Per ogni $n\geq b, P(n)$" dove $b$ è un intero fissato (individua $P(n)$, individua $b$).
>
> 2. Scrivi la parola "Passo Base" e mostra che $P(b)$ è vera.
> 3. Scrivi la parola "Passo Induttivo".
> 4. Esprimi in modo chiaro l'Ipotesi Induttiva: "Assumiamo che $P(k)$ è vera per un qualsiasi intero $k\geq b$".
> 5. Scrivi in maniera esplicita cosa occorre provare, cioè cosa afferma $P(k+1)$.
> 6. Prova che $P(k+1)$ è vera **usando** l'ipotesi che $P(k)$ è vera.
> Controlla che la prova è corretta **per ogni** $k\geq b$, incluso $b$. 
> 7.  Esprimi chiaramente la conclusione del passo induttivo, dicendo "questo completa il passo induttivo".
> 8. Dopo aver completato il passo base ed il passo induttivo, esprimi chiaramente la conclusione dicendo che, per il principio di induzione, $P(n)$ è vera per tutti gli interi $n$, con $n\geq b$.

---
> [!example]- <font color="orange">Esempio</font>
> Vogliamo dimostrare la seguente formula per ogni $n\in\mathbb{N},\ n>0$.
> >$$1\ +\ 2\ +\ ...\ +\ n\ =\frac{n\cdot(n+1)}{2}$$
>
**BASE**: Per $n=1$ risulta $$1\ +\ 2\ +\ ...\ +\ n\ =\textcolor{#CC241D}1$$ $$\frac{n\cdot(n+1)}{2}=\frac{1\cdot(1+1)}{2}=\textcolor{#CC241D}1$$
e quindi il passo base è dimostrato.
>
*Ipotesi Induttiva*: Sia $n\geq 1$, supponiamo che la seguente $$1\ +\ 2\ +\ ...\ +\ n\ =\frac{n\cdot(n+1)}{2}$$
sia vera.
>
**PASSO INDUTTIVO**:  Proviamo che sia vera $$1\ +\ 2\ +\ ...\ +\ n\ +\ (n+1)=\color{#CC241D}\frac{(n+1)\cdot((n+1)+1)}{2}$$ abbiamo, per via della *Ipotesi Induttiva* (*i.i.*), che la somma da $1$ ad $n$ è uguale a $\frac{n\cdot(n+1)}{2}$, risulta $$=\frac{n\cdot(n+1)}{2}+(n+1)$$ $$=\frac{n\cdot(n+1)+2\cdot(n+1)}{2}= \frac{(n+1)\cdot(n+2)}{2}$$
$$=\color{#CC241D}\frac{(n+1)\cdot((n+1)+1)}{2}$$
e quindi il passo induttivo è dimostrato.

^2fedf4



___
[[000 Indice MMI|↖ Ritorna all'indice ↖]]

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

