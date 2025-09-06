---
aliases: Relazione di Equivalenza
tags:
  - corsi/matematica/matematica_discreta
paragrafo: Relazioni - Equivalenza
cssclasses:
  - 
---
>Una **Relazione di Equivalenza** è un tipo di [[025 Relazioni Binarie|relazione binaria]] su un insieme che *soddisfa tre importanti proprietà matematiche*: 
>- **Riflessività**;
>- **Simmetria**;
>- **Transitività**. 

### Proprietà Riflessiva
>Data una relazione $R \subseteq A\times A$, ogni elemento dell'insieme è *in relazione con se stesso*. 

> [!quote] Definizione Formale
> $\forall a \in A, (a,a)\in R \iff \color{#CC241D}\forall a\in A, aRa$

Non è riflessiva se $\exists x\in A: x\not Rx$.

> [!tip] Riconoscere la proprietà riflessiva
> Se *TUTTI* gli elementi dell'insieme $S$ si trovano in modo che $(x, x), (x_n, x_n)\in R$, allora $R$ è *RIFLESSIVA*.
>Il resto delle coppie non riflessive in $R$ non contano.

### Proprietà Simmetrica
>Data una relazione $R \subseteq A\times A$, se due elementi $a$ e $b$ sono in relazione, allora *accade anche l'opposto* ($b$ è in relazione con $a$).

> [!quote] Definizione Formale
> $a,b \in A, (a,b)\in R \implies (b,a)\in R \iff \color{#CC241D}a,b\in A, aRb \implies bRa$

> [!tip] Riconoscere la proprietà simmetrica
>Si notano *SOLO* gli elementi dell'insieme $S$ che si trovano nella relazione $R$.
>- Le coppie riflessive, come $(x, x), (x_n, x_n)\in R$, sono sempre *SIMMETRICHE*. Bisogna però verificare se tutte le coppie non riflessive sono simmetriche.
>- Se tra le coppie abbiamo $(x,y)\in R$ per essere *simmetrica* dobbiamo anche avere *$(y,x)\in R$*.

### Proprietà Transitiva
>Data una relazione $R \subseteq A\times A$, se tre elementi $a$, $b$ e $c$ sono in relazione e $a$ è in relazione con $b$ e $b$ è in relazione con $c$, allora *$a$ è in relazione con $c$*.

> [!quote] Definizione Formale
> $a,b,c \in A, (a,b),(b,c)\in R \implies (a,c)\in R \iff \color{#CC241D}a,b,c\in A, aRb \land bRc \implies aRc$

> [!tip] Riconoscere la proprietà transitiva
>Si notano *SOLO* gli elementi dell'insieme $S$ che si trovano nella relazione $R$.
>- Se la relazione è un [[001 Insieme#Singleton|Singleton]] con una sola coppia, allora è *TRANSITIVA*.
>- Se si trova una situazione del tipo: $(x,y), (y,x)\in R$, per vedere se $R$ è *transitiva* dobbiamo stabilire se *$(x,x),(y,y)\in R$*.
>- Se tra le coppie abbiamo $(x,y),(y,z)\in R$ per essere *transitiva* dobbiamo anche avere *$(x,z)\in R$*.

---
> [!example]- <font color="orange">Esempio</font>
>$R=\{(x,y)\in \mathbb{Z}\times \mathbb{Z}:\space|x-8|=|y-8|\}$
>Quindi $xRy\iff |x-8|=|y-8|$.
>
>$R$ è:
>*Riflessiva*: 
>$\forall x\in \mathbb{Z},xRx\iff |x-8|=|x-8|$ è vera.
>*Simmetrica*: $xRy\iff |x-8|=|y-8| \Rightarrow |y-8|=|x-8|\Rightarrow yRx$ è vera. 
>*Transitiva*:
>$\begin{matrix}
>xRy\\
>\land\\
>yRz\\
>\end{matrix} \Rightarrow$ $\begin{matrix}
>|x-8|=|y-8|\\
>\land\\
>|y-8|=|z-8|\\
>\end{matrix} \Rightarrow |x-8|=|z-8| \Rightarrow xRz$ è vera.

> [!example]- <font color="orange">Esempi</font>
>$A=\{a,b,c\}$
>
>| Relazione       | Riflessiva | Simmetrica | Transitiva |
>| --------------- | ---------- | ---------- | ---------- |
>| $R_1=\{(a,b)\}$ | no         | no         | SI         | 
>
>Perché:
>Per essere Riflessiva: mancano $(a,a), (b,b), (c, c)$;
>Per essere Simmetrica: manca $(b,a)$;
>È Transitiva: abbiamo $(a,b)$.
>
>---
>
>| Relazione                   | Riflessiva | Simmetrica | Transitiva |
>| --------------------------- | ---------- | ---------- | ---------- |
>| $R_2=\{(a,b),(b,a),(a,a)\}$ | no         | SI         | no         |
>
>Perché:
>Per essere Riflessiva: mancano $(b,b), (c,c)$;
>È Simmetrica: abbiamo $(a,b), (b,a)$;
>Non è Transitiva: 
>$(a,b),(b,a)\Rightarrow (a,a)$,
>$(b,a),(a,b)\not\Rightarrow (b,b)$, manca $(b,b)$.
>
>---
>
>| Relazione                         | Riflessiva | Simmetrica | Transitiva |
>| --------------------------------- | ---------- | ---------- | ---------- |
>| $R_3=\{(a,b),(b,a),(a,a),(b,b)\}$ | no         | SI         | SI         |
>
>Perché:
>Per essere Riflessiva: manca $(c,c)$;
>È Simmetrica: abbiamo $(a,b), (b,a)$;
>È Transitiva: 
>$(a,b),(b,a)\Rightarrow (a,a)$
>$(b,a),(a,b)\Rightarrow (b,b)$.
>
>---
>
>| Relazione                         | Riflessiva | Simmetrica | Transitiva |
>| --------------------------------- | ---------- | ---------- | ---------- |
>| $R_4=\{(a,a),(b,b),(c,c),(a,b)\}$ | SI         | no         | SI         |
>
>Perché:
>È Riflessiva: abbiamo $(a,a), (b,b), (c,c)$;
>Per essere Simmetrica: manca $(b,a)$;
>È Transitiva: 
>$(a,a),(a,b)\Rightarrow (a,b)$
>$(a,b),(b,b)\Rightarrow (a,b)$.
>
>---
>
>| Relazione                               | Riflessiva | Simmetrica | Transitiva |
>| --------------------------------------- | ---------- | ---------- | ---------- |
>| $R_5=\{(a,a),(b,b),(c,c),(a,b),(b,a)\}$ | SI         | SI         | SI         | 
>
>Perché:
>È Riflessiva: abbiamo $(a,a), (b,b), (c,c)$;
>È Simmetrica: abbiamo $(a,b), (b,a)$;
>È Transitiva: 
>$(a,a),(a,b)\Rightarrow (a,b)$
>$(a,b),(b,b)\Rightarrow (a,b)$
>$(b,a),(a,a)\Rightarrow (b,a)$.

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
- Pagina Libro: 79