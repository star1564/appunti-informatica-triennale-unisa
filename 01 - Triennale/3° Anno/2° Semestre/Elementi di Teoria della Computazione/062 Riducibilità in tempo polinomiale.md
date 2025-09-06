---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Riducibilità di Tempo Polinomiale
cssclasses:
    - 
---

## Definizioni

### Riduzione in tempo polinomiale

> Siano $A$ e $B$ due linguaggi sull'alfabeto $\Sigma$. Una **[[045 Riducibilità|riduzione]] di tempo polinomiale** $f$ da $A$ in $B$ è una [[061 Funzioni calcolabili in tempo polinomiale|funzione calcolabile in tempo polinomiale]] $f:\Sigma^*\to \Sigma^*$ tale che
> $$\forall w\in\Sigma^*,\quad w\in A\iff f(w)\in B$$

> [!note]+ Nota
> Nella definizione di riduzione di tempo polinomiale NON è richiesto che $f$ sia una funzione [[020 Applicazioni Iniettive|iniettiva]] o [[019 Applicazioni Suriettive|suriettiva]].

### Riducibilità in tempo polinomiale

> Un linguaggio $A\subseteq\Sigma^*$ è **riducibile in tempo polinomiale** a un linguaggio $B\subseteq\Sigma^*$ se esiste una _[[#Riduzione in tempo polinomiale|riduzione di tempo polinomiale]]_ da $A$ in $B$. Si segna con
> $$\color{#CC241D}A\leq_p B$$

La riducibilità in tempo polinomiale è l'analogo _efficiente_ della [[045 Riducibilità#Riducibilità mediante funzione|Riducibilità mediante funzione]].

> [!note]+ Nota
> Se $A$ è un linguaggio su un alfabeto $\Sigma$ e $A$ è associato a un problema di decisione $\mathbb{P}_D$, le stringhe $w$ in $\Sigma^*$ si dividono in tre gruppi:
>
> 1.  $w$ è la codifica di un'istanza di $\mathbb{P}_D$ per la quale $\mathbb{P}_D$ ammette risposta "_si_" (e quindi $w \in A$)
> 2.  $w$ è la codifica di un'istanza di $\mathbb{P}_D$ per la quale $\mathbb{P}_D$ ammette risposta "_no_" (e quindi $w \not\in A$)
> 3.  $w$ _non è la codifica_ di un'istanza di $\mathbb{P}_D$ (e quindi $w \not\in A$)
>
> In generale nelle prove di riduzione di tempo polinomiale da $A$ in un altro linguaggio $B$, vengono considerate solo le stringhe dei _primi due gruppi_ e si assume implicitamente che la riduzione $f$ associa alle stringhe $w$ del terzo gruppo una stringa $f(w)$ che non è in $B$.


## Teoremi

> [!quote]+ Teorema 1
>
> > Se $A\leq_p B$ e $B\in P$ , allora $A\in P$.
>
> Ovvero che se un linguaggio $A$ è riducibile in tempo polinomiale a un linguaggio $B$ e già si sa che $B$ ha un decisore di tempo polinomiale, si ottiene un decisore di tempo polinomiale per il linguaggio originale $A$.
>
> Abbiamo che:
>
> -   $B\in P$, quindi esiste un algoritmo $M$ di complessità $O(m^t)$ che decide $B$.
> -   $A\leq_p B$, ovvero che abbiamo una riduzione di tempo polinomiale $f$ da $A$ in $B$ e abbiamo un algoritmo $F$ di complessità $O(n^k)$ che calcola la funzione $f$.
>
> Consideriamo il seguente algoritmo.
>
> > [!note] Descrizione di $N$
> > Sull'input $w$:
> >
> > 1. Simula $F$ su $w$ e calcola $f(w)$
> > 2. Simula $M$ sull'input $f(w)$ per decidere se $f(w)\in B$
> > 3. $N$ accetta $w$ se $M$ accetta $f(w)$, altrimenti rifiuta
> >
> > $$N:w\to \boxed{\boxed{F}\to f(w) \to \boxed{M}}$$
>
> $N$ decide $A$. Infatti, $N$ si ferma su $w$ se si fermano $F$ ed $M$. Ora, per ogni $w$, $F$ si ferma con $f(w)$ sul nastro e per ogni $w$, $M$ si ferma su $f(w)$ perché $M$ è un decider.
>
> Inoltre $N$ riconosce $A$. Infatti:
>
> $w\in \mathcal{L}(N)\iff f(w)\in \mathcal{L}(M)$ [per la definizione di $N$] 
> $\quad\quad\quad\quad\ \iff f(w)\in B$ [perché $M$ decide $B$] 
> $\quad\quad\quad\quad\ \iff w\in A$ [perché $f$ è una riduzione polinomiale di $A$ in $B$]
>
> $N$ è un algoritmo polinomiale in $n = |w|$. Infatti, $F$ calcola $f (w)$ in $O(n^k)$ passi (dove il primo passo dell'algoritmo è polinomiale). Quindi $F$ calcola $f(w)$ in $q(n)$
> passi, per qualche polinomio $q$.
> In particolare, questo significa $|f (w)| \leq q(n)$, perché _la lunghezza dell'output di $F$ è limitata dalla complessità di tempo di $F$_.
>
> Al secondo passo, $M$ viene eseguito sull'input $f (w)$ e si arresterà dopo $p(|f (w)|) \leq p(q(n))$ passi, per qualche polinomio $p$ (il secondo passo dell'algoritmo è polinomiale, una composizione dei due polinomi).
>
> In conclusione $N$ ha complessità polinomiale, quindi $A \in P$.

^ad2b8c

> [!quote]+ Proprietà transitiva di $\leq_p$
>
> > Se $A\leq_p B$ e $B\leq_p  C$ , allora $A\leq_p C$.
>
> Per ipotesi:
>
> -   Esiste una riduzione di tempo polinomiale $f:\Sigma^*\to \Sigma^*$ di $A$ in $B$
> -   Esiste una riduzione di tempo polinomiale $g:\Sigma^*\to\Sigma^*$di $B$ in $C$
>
> Consideriamo la [[023 Applicazioni Composte|composizione]] $g \circ f:\Sigma^*\to\Sigma^*$delle funzioni $f$ e $g$, definita da $$(g \circ f )(w) = g(f(w))$$
>
> Risulta, per ogni $w \in \Sigma^*$ che
> $w \in A \iff f (w) \in B$ [perché $f$ è una riduzione polinomiale di $A$ in $B$] 
> $\quad\quad\quad\iff g(f (w)) \in C$ [perché $g$ è una riduzione polinomiale di $B$ in $C$]
>
> Inoltre la funzione $g \circ f$ è una funzione calcolabile in tempo polinomiale.
>
> Infatti, sia $F$ l'algoritmo di complessità $O(n^k)$ che calcola la funzione $f$ e sia $G$ l'algoritmo di complessità $O(m^t)$ che calcola la funzione $g$. Consideriamo il seguente algoritmo $GF$.
>
> > [!note] Descrizione di $GF$
> > Sull'output $w$:
> >
> > 1. Simula $F$ su $w$ e calcola $f(w)$
> > 2. Simula $G$ su $f(w)$ e calcola $g(f(w))$
> > 3. Fornisce in output l'output di $G$
>
> L'algoritmo $GF$ calcola $g \circ f$ perché:
>
> 1.  Esegue prima $F$ su $w$ calcolando $f(w)$ (primo passo dell'algoritmo)
> 2.  Poi esegue $G$ su $f(w)$ (secondo passo dell'algoritmo) fornendo quindi in output $g(f (w))$.
>
> GF è un algoritmo polinomiale in $n = |w|$. Infatti, $F$ calcola $f(w)$ in $O(n^k)$ passi (il primo passo dell'algoritmo è polinomiale). Quindi $F$ calcola $f (w)$ in $q(n)$ passi, per qualche polinomio $q$. In particolare, questo significa $|f (w)| \le q(n)$ (perché _la lunghezza dell'output di $F$ è limitata dalla complessità di tempo di $F$_).
>
> Al secondo passo, $G$ viene eseguito sull'input $f(w)$ calcolando $g(f (w))$, poi si arresterà dopo $p(|f (w)|) \le p(q(n))$ passi, per qualche polinomio $p$ (il secondo passo dell'algoritmo è polinomiale, una composizione dei due polinomi).
>
> In conclusione $GF$ ha complessità polinomiale. Quindi $g \circ f$ è una riduzione di tempo polinomiale di $A$ in $C$.

^6ba29e

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
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva \to" + "]]"
		toDisplay.push(nextLink)
	} else if (prevAndNext[0][1] == undefined){
		// Se è l'ultima pagina aggiungi solo il precedente
		let prevLink = "[[" + sortedPrevAndNext[0] + "|" + "← Nota Precedente" + "]]"
		toDisplay.push(prevLink)
	} else {
		let nextLink = "[[" + sortedPrevAndNext[1] + "|" + "Nota Successiva \to" + "]]"
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
