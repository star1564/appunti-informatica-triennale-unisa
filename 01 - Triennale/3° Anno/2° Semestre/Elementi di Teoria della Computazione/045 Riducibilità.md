---
aliases:
    - Funzioni non calcolabili
    - Riducibilità mediante funzione
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Riducibilità
cssclasses:
    - 
---

## Descrizione Informale

Il metodo principale per dimostrare che alcuni problemi sono _computazionalmente irrisolvibili_ si chiama **Riducibilità**.

> Una **Riduzione** è un modo di _convertire un problema in un altro problema_, in modo tale che una soluzione al secondo problema _può essere usata_ per risolvere il primo problema. La riducibilità coinvolge sempre due problemi, $A$ e $B$.

Se $A$ si riduce a $B$:

-   possiamo _usare una soluzione di $B$_ per risolvere $A$
-   ma, se _$A$ non si può risolvere_, allora neanche $B$ si può risolvere.

Quando $A$ è riducibile a $B$, allora il problema associato ad $A$ "_non è più difficile_" di quello associato a $B$, perché una soluzione per $B$ offre una soluzione ad $A$. Infatti, stiamo guardando $A$ in una forma diversa $B$.

Quindi, si può notare che un problema $P$ è [[042 Linguaggi Indecidibili#Definizione Problema Indecidibile|indecidibile]] con tre modi:

-   Supporre l'esistenza di una Macchina di Turing che decide il linguaggio associato a $P$ e provare che questo conduce ad una contraddizione.
-   Considerare un problema $P'$ di cui sia nota l'indecidibilità del linguaggio associato e dimostrare che $P'$ "non è più difficile" del problema in questione $P$.
-   [[050 Teorema di Rice|Teorema di Rice]]


## Definizioni

### Riduzione

> Una **Riduzione** da un linguaggio $A\subseteq\Sigma^*$ a un linguaggio $B\subseteq\Sigma^*$ è una _[[044 Funzioni Calcolabili|funzione calcolabile]]_ $f:\Sigma^* \to \Sigma^*$ tale che
> $$\forall w\in\Sigma^*\quad w\in A\Leftrightarrow f(w)\in B$$
> La funzione $f$ è chiamata una _riduzione da $A$ in $B$_.

![[Pasted image 20240521121139.png|700]]

### Riducibilità mediante funzione

> Un linguaggio $A\subseteq\Sigma^*$ è **Riducibile mediante funzione** a un linguaggio $B\subseteq\Sigma^*$, il che si indica con $\color{#CC241D}A\leq_m B$, se esiste una _[[#Riduzione|riduzione]] da $A$ in $B$_.

> [!example]- <font color="orange">Esempio</font>
> Sia $\Sigma = \{0, 1\}$. Denotiamo:
>
> -   $\langle n \rangle$, la rappresentazione binaria di $n \in \mathbb{N}$.
> -   $EVEN = \{w \in \Sigma^∗\ |\ w = \langle n \rangle, n \in \mathbb{N}\ \ pari\}$, l'insieme delle stringhe binarie che rappresentano numeri naturali pari.
> -   $ODD = \{w \in \Sigma^*\ |\ w = \langle n \rangle, n \in \mathbb{N}\ \ dispari\}$, l'insieme delle stringhe binarie che rappresentano numeri naturali dispari.
>
> Definiamo la seguente Macchina di Turing $INCR$, che ha il compito di incrementare di 1 il numero rappresentato dalla stringa input $w$ in binario.
>
> $$w\to \boxed{INCR}\to \begin{cases} w,\quad\quad\quad\quad\quad\text{se }w\notin(1\ \Sigma^*\cup \{0\}) \\ w' = \langle n+1 \rangle, \text{ se } w = \langle n \rangle \end{cases}$$
> Ovvero:
>
> -   Se $w$ non appartiene a $1\ \Sigma^*$ o non è uguale a $\{0\}$, restituisce $w$ senza modificarlo. Questo accade quando $w$ è vuoto oppure contiene caratteri non binari (che non sono 0 o 1).
> -   Se $w$ è la rappresentazione binaria di un numero naturale $n$, allora calcola $w'=\langle n+1 \rangle$, ossia la rappresentazione binaria di $n+1$.
>
> Ad esempio:
>
> -   $w=0$:
>     -   $w'=1$, ovvero $0 + 1 = 1$
> -   $w=1$:
>     -   $w'=10$, ovvero $1 + 1 = 2$
> -   $w=1011$:
>     -   $w'=1100$, ovvero $11 + 1 = 12$
> -   $w=abc$:
>     -   Restituisce $abc$ senza modificarlo.
> -   $w=\epsilon$ (stringa vuota):
>     -   Restituisce $\epsilon$ senza modificarlo.
>
> Si può costruire una Macchina di Turing capace di calcolare questa funzione.
>
> Abbiamo quindi definito una funzione calcolabile
> $$f:w\in\Sigma^*\to w'\in\Sigma^*$$
> tale che $$\color{#CC241D}w=\langle n \rangle\in EVEN \Longleftrightarrow f(w)=w'=\langle n+1 \rangle \in ODD$$
>
> Questo perché:
>
> -   Se $w\in EVEN$, allora $w=\langle n \rangle$ per qualche $n$ pari. In tal caso $f(w)=\langle n+1 \rangle\in ODD$, quindi $\color{#61AFEF}w\in EVEN \Longrightarrow f(w)=w'=\langle n+1 \rangle\in ODD$
> -   Se $w\not\in EVEN$, allora $w$ rappresenta un numero dispari $n$ oppure non rappresenta un numero.
>
>     -   Se $w$ rappresenta un numero dispari, allora $f(w)=\langle n+1 \rangle\in EVEN$
>     -   Se $w$ non rappresenta un numero, allora $f(w)=w\not\in ODD$
>
>     Quindi $w\not\in EVEN \Longrightarrow f(w)=w'=\langle n+1 \rangle\not\in ODD$, che per via del [[010 Inverso, Opposto e Contronominale|Contronominale]] è equivalente a $\color{#61AFEF} f(w)=w'=\langle n+1 \rangle\in ODD \Longrightarrow w\in EVEN$
>
> La funzione $f$ (definita su tutto $\Sigma^*$) è una riduzione di $EVEN$ a $ODD$. Pertanto, il linguaggio $EVEN$ **è riducibile mediante funzione** a $ODD$. Ovvero che $$\color{#CC241D}EVEN \leq_m ODD$$
>
> Possiamo notare che $EVEN$ "non è più difficile" di $ODD$.
> Supponiamo di avere una Macchina di Turing $R$ che decide se una stringa appartiene al linguaggio $ODD$. Vogliamo _costruire_ una macchina di Turing $S$ che decida se una stringa appartiene al linguaggio $EVEN$.
>
> $$S : w \to \boxed{INCR} \to w' \to \boxed{R}$$
>
> La Macchina di Turing $S$ utilizzerà come "sottoprogrammi" $R$ ed $INCR$. Funziona come segue:
>
> -   Prende in input una stringa $w$.
> -   Applica la funzione $INCR$ a $w$ per ottenere $w'$, che rappresenta $\langle n+1 \rangle$.
> -   Passa $w'$ alla macchina $R$.
> -   La macchina $R$:
>     -   Accetta $w'$ (cioè $w'\in ODD$), allora $w\in EVEN$.
>     -   Rifiuta $w'$ (cioè $w'\notin ODD$), allora $w\notin EVEN$.
>
> Supponiamo che $EVEN$ fosse indecidibile e che $ODD$ fosse decidibile. Allora esisterebbe una macchina $R$ che decide $ODD$.
> Ma la costruzione della macchina $S$ implica che $EVEN$ sarebbe decidibile se $ODD$ fosse decidibile.
> Questo contraddice l'assunzione che $EVEN$ sia indecidibile. Pertanto, se $EVEN$ è indecidibile, anche $ODD$ deve essere indecidibile.

> [!example]- <font color="orange">Esempio di Funzione NON Calcolabile</font>
> Consideriamo $A_{TM}$ e $B=\{ab\}$. Consideriamo la funzione $f : \Sigma^* \to \Sigma^*$, dove $a,b,\in\Sigma$, così definita:
> $$f(y)=\begin{cases} ab,\  \text{ se } y= \langle M,w \rangle\in A_{TM} \\ a,\quad  \text{altrimenti} \end{cases}$$
>
> Quindi $f$ è una funzione tale che
>
> -   $f(y)= a$ se $y$ non è della forma $\langle M,w \rangle$, oppure se $y=\langle M,w \rangle$ con $\langle M,w \rangle\notin A_{TM}$
> -   $f(y)=ab$ se $y=\langle M,w \rangle$ con $\langle M,w \rangle\in A_{TM}$
>
> Quindi per ogni $y\in\Sigma^*$ abbiamo che
> $$y\in A_{TM}\Leftrightarrow f(y)\in\{ab\}$$
>
> La funzione $f$ non è calcolabile.
>
> Supponiamo, per assurdo, che $f$ sia calcolabile. Allora esisterebbe una macchina di Turing $F$ che, dato un input $y$, calcola $f(y)$.
> Questo significa che $F$ decide se $y \in A_{TM}$ e produce
>
> -   $ab$ se $y \in A_{TM}$
> -   $a$ se $y \notin A_{TM}$
>
> Se $F$ esistesse, potremmo costruire una macchina di Turing $R$ che decide $A_{TM}$.
>
> > [!note] Descrizione di $R$
> > Sull'input $\langle M, w \rangle$:
> >
> > 1. Esegui $F$ su $\langle M, w \rangle$
> > 2. ➜ Se $F(\langle M, w \rangle) = ab$, accetta.
> >    ➜ Se $F(\langle M, w \rangle) = a$, rifiuta.
>
> Questa macchina $R$ deciderebbe $A_{TM}$ perché:
>
> -   Se $\langle M, w \rangle \in A_{TM}$, allora $F(\langle M, w \rangle) = ab$, quindi $R$ accetta.
> -   Se $\langle M, w \rangle \notin A_{TM}$, allora $F(\langle M, w \rangle) = a$, quindi $R$ rifiuta.
>
> Sappiamo che $A_{TM}$ è indecidibile, il che significa che non esiste una macchina di Turing che decida $A_{TM}$. Questo implica che non può esistere una macchina di Turing $R$ come descritta sopra. Pertanto, $F$ non può esistere e, di conseguenza, la funzione $f$ non è calcolabile.


## Riduzioni importanti

In questi teoremi si prova l'esistenza di riduzioni da $A_{TM}$ (o da un altro linguaggio indecidibile) ad alcuni linguaggi $B$ associati a problemi di decisione sulle macchine di Turing. Una conseguenza importante di tali teoremi è che **ognuno di questi linguaggi $B$ è indecidibile**.

Inoltre, quando descriviamo una macchina di Turing che calcola una riduzione da $A$ in $B$, assumiamo che agli input che _non sono della forma corretta_ sia associata una stringa _al di fuori_ di $B$.

Più precisamente, se $A$ è il linguaggio associato a un problema di decisione, definiamo la riduzione solo sulle codifiche delle [[033 Istanze di un Problema di Decisione|istanze del problema]]. Questo _non_ significa "sugli elementi di A".

> [!example]- <font color="orange">Esempio</font>
> Se $A = A_{TM}$, definiremo la riduzione (e la macchina di Turing che calcola la riduzione) sulle stringhe della forma $\langle M,w \rangle$, dove $M$ è una Macchina di Turing e $w$ è una stringa. Questo _non_ significa "sugli elementi di ATM".

I linguaggi $B$ indecidibili sono:

-   [[046 Riduzione di HALT|HALT]]
-   [[047 Riduzione del Complemento di E|Complemento di E]]
-   [[048 Riduzione di REGULAR|REGULAR]]
-   [[049 Riduzione di EQ|EQ ed il suo Complemento]]


## Teoremi

> [!quote]+ Proprietà transitiva di $\leq_m$
>
> > Se $A\leq_m B$ e $B\leq_m  C$ , allora $A\leq_m C$.

> [!quote]+ Teorema 1
>
> > $A\leq_m B$ se e solo se $\overline{A}\leq_m \overline{B}$.
>
> Per ipotesi $A\leq_m B$, quindi esiste una riduzione $f$ di $A$ in $B$. Poiché $f$ è una riduzione, $f$ è calcolabile, inoltre
> $$\forall w\in\Sigma^*\quad w\in A\Leftrightarrow f(w)\in B$$
>
> Proviamo che $f$ è anche una riduzione da $\overline{A}$ in $\overline{B}$.
>
> Abbiamo
> $$\forall w\in\Sigma^*\quad w\notin A\Leftrightarrow f(w)\notin B$$
> che equivale [[008 Equivalenza Bicondizionale#^63c4d0|per via dell'equivalenza con il negato]] a
> $$\forall w\in\Sigma^*\quad w\in \overline{A}\Leftrightarrow f(w)\in \overline{B}$$
> Quindi, per definizione, $f$ è una riduzione da $\overline{A}$ in $\overline{B}$.

^55b02f

> [!quote]+ Teorema 5.22 [Sipser]
>
> > Se $A\leq_m B$ e $B$ è decidibile, allora $A$ è decidibile.
>
> Sia $M_B$ una Macchina di Turing che decide $B$, sia $f$ una funzione di riduzione da $A$ in $B$ e sia $M_f$ una Macchina di Turing che calcola $f$.
>
> Consideriamo la Macchina di Turing $M_A$.
>
> > [!note] Descrizione di $M_A$ 
> > $$M_A:w\to \boxed{M_f}\to f(w) \to \boxed{M_B}$$
> > Sull'input $w$:
> >
> > 1. Simula $M_f$ e calcola $f(w)$
> > 2. Simula $M_B$ su $f(w)$
> > 3. ➜ Se $M_B$ accetta $f(w)$, accetta.
> >    ➜ Se $M_B$ rifiuta $f(w)$, rifiuta.
>
> $M_A$ decide $A$, quindi è un [[024 Macchine Decisionali|decisore]].
>
> Infatti $M_A$ si ferma su $w$ se si fermano $M_f$ ed $M_B$. Ora, per ogni $w$, $M_f$ si ferma con $f(w)$ sul nastro e per ogni $w$, $M_B$ si ferma su $f(w)$ perché $M_B$ è un decider.
>
> Inoltre $M_A$ riconosce $A$. Infatti:
> $$w\in \mathcal{L}(M_A)\Leftrightarrow f(w)\in \mathcal{L}(M_B) \text{ [per la definizione di }M_A\text{]}$$ 
> $$\quad\quad\quad \Leftrightarrow f(w)\in B \text{ [perché }M_B\text{ riconosce }B \text{]}$$ 
> $$\quad\quad\quad\quad\quad \Leftrightarrow w\in A \text{ [per la definizione di riduzione]}$$

^546381

> [!example]- Nota sul Teorema 5.22
>
> > Se $A$ è decidibile _non possiamo dedurre nulla_ su $B$
>
> Ad esempio, consideriamo $A=\{ab\}$ e $A_{TM}$.
> $A = \{ab\}$ è decidibile e $A_{TM}$ è indecidibile. Ma possiamo provare che $\{ab\} \leq_m A_{TM}$.
>
> Sia $M$ la Macchina di Turing tale che $\mathcal{L}(M)=\mathcal{L}(a^*)$. Consideriamo la funzione $f:\Sigma^*\to\Sigma^*$, dove $a,b\in\Sigma$, così definita:
> $$f(y)=\begin{cases} \langle M,a \rangle, \text{ se } y=b \\ \langle M,b \rangle, \text{ altrimenti} \end{cases}$$
>
> Ovviamente $\langle M,a \rangle\in A_{TM}$, mentre $\langle M,b \rangle\notin A_{TM}$. Vogliamo dimostrare che $f$ è una riduzione da $\{ab\}$ in $A_{TM}$.
>
> La funzione $f$ è calcolabile. Infatti possiamo costruire una Macchina di Turing $F$ che calcola $f$ sull'input $y$:
>
> -   Se $y=ab$, cancella $ab$, scrive la stringa $\langle M,a \rangle$ e si ferma
> -   Se $y\neq ab$, cancella $y$, scrive la stringa $\langle M,b \rangle$ e si ferma
>
> Inoltre:
>
> -   $y\in\{ab\}\Rightarrow y=ab\Rightarrow f(y)=\langle M,a \rangle \in A_{TM}$
> -   $y\notin\{ab\}\Rightarrow y\neq ab\Rightarrow f(y)=\langle M,b \rangle \notin A_{TM}$
>
> Quindi, per ogni stringa $y$ abbiamo che $$y\in\{ab\}\Leftrightarrow f(y)\in A_{TM}$$
> In conclusione, $\{ab\} \leq_m A_{TM}$.

> [!quote]+ Teorema 5.28 [Sipser]
>
> > Se $A\leq_m B$ e $B$ è [[023 Linguaggio Turing-Riconoscibile#^5b96ec|Turing-Riconoscibile]], allora $A$ è Turing-Riconoscibile.
>
> Sia $M_B$ una Macchina di Turing che decide $B$, sia $f$ una funzione di riduzione da $A$ in $B$ e sia $M_f$ una Macchina di Turing che calcola $f$.
>
> Consideriamo la stessa Macchina di Turing $M_A$ di prima.
>
> $M_A$ [[023 Linguaggio Turing-Riconoscibile|riconosce]] $A$. Infatti:
> $$w\in \mathcal{L}(M_A)\Leftrightarrow f(w)\in \mathcal{L}(M_B) \text{ [per la definizione di }M_A\text{]}$$ 
> $$\quad\quad\quad \Leftrightarrow f(w)\in B \text{ [perché }M_B\text{ riconosce }B \text{]}$$ 
> $$\quad\quad\quad\quad\quad \Leftrightarrow w\in A \text{ [per la definizione di riduzione]}$$

^a8c89d

> [!summary]+ Corollario 1
>
> > Se $A\leq_m B$ e $A$ è indecidibile, allora $B$ è indecidibile.
>
> Se $B$ fosse decidibile, lo sarebbe anche $A$ per via del [[#^546381|Teorema 5.22]].

> [!summary]+ Corollario 2
>
> > Se $A\leq_m B$ e $A$ [[040 Linguaggi Non Turing-Riconoscibili|NON è Turing-Riconoscibile]], allora $B$ non è Turing-Riconoscibile.
>
> Se $B$ fosse Turing-Riconoscibile, lo sarebbe anche $A$ per via del [[#^a8c89d|Teorema 5.28]].

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

-   [lydia](https://www.youtube.com/watch?v=U4yGQp5aCTM)
