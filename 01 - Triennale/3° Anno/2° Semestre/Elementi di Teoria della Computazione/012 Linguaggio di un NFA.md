---
aliases:
    - Linguaggio Riconosciuto da un NFA
    - Equivalenza NFA-DFA
    - Subject Construction
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Non Deterministici
cssclasses:
    - 
---

> Sia $A=(Q,\Sigma, \delta,q_0,F)$ un [[009 Automi Finiti Non Deterministici|automa finito non deterministico]]. Il **Linguaggio Accettato** (o _riconosciuto_) da $A$ è il seguente
> $$\color{#CC241D}\mathcal{L}(A)=\{w\in\Sigma^*\ |\ \hat\delta(q_0,w)\cap F\neq \emptyset\}$$

Ovvero, è l'insieme di quelle stringhe che partono dallo stato iniziale e raggiungono _uno degli stati finali_. Si mette l'intersezione diverso dall'insieme vuoto perché la [[011 Funzione di Transizione Estesa NFA|funzione di transizione estesa]] ritorna un'insieme di stati e basta che almeno uno sia finale per dire che la stringa è accettata.

-   Se $|F|=0$, allora $\mathcal{L}(M)=\emptyset$.
-   Se $Q=F$, non è detto che l'NFA accetta tutte le stringhe, infatti dipende da come è composto l'automa.
-   Se $|Q|=1$, dipende dalla funzione di transizione.
-   Il linguaggio accetta la parola vuota quando si parte dallo stato iniziale e si raggiunge (tramite $\epsilon$-transizioni) un altro stato che è finale, ovvero che la [[010 ε-chiusura negli NFA|ε-chiusura]] $E(q_0)$ deve contenere almeno uno stato finale.


## Equivalenza NFA-DFA

> Per ogni automa finito non deterministico (NFA) esiste un automa finito deterministico (DFA) **equivalente** che _accetta lo stesso linguaggio_.

> [!example]- <font color="orange">Esempio</font>
> ![[Pasted image 20240320180911.png|700]]

### Teorema DFA $\to$ NFA

> Per ogni [[004 Automi Finiti Deterministici|DFA]] $M=(Q_M,\Sigma,\delta_M,q_M,F_M)$ esiste un [[009 Automi Finiti Non Deterministici|NFA]] $N=(Q_N,\Sigma,\delta_N,q_N,F_N)$ tale che $\mathcal{L}(M)=\mathcal{L}(N)$.

> [!quote] Dimostrazione
> Basta considerare:
>
> -   $Q_N=Q_M$
> -   $q_N=q_M$
> -   $F_N=F_M$
> -   $\delta_N:Q_M\times\Sigma_\epsilon\to \mathcal{P}(Q_M)$, dove $\forall q\in Q_N, \forall a \in\Sigma$ abbiamo che: $$\delta_N(q,a)=\{\delta_M(q,a)\}$$
>
> È evidente che $\mathcal{L}(M)=\mathcal{L}(N)$.

### Teorema NFA $\to$ DFA

> Per ogni [[009 Automi Finiti Non Deterministici|NFA]] $N=(Q_N,\Sigma,\delta_N,q_N,F_N)$ esiste un [[004 Automi Finiti Deterministici|DFA]] $M=(Q_M,\Sigma,\delta_M,q_M,F_M)$ tale che $\mathcal{L}(M)=\mathcal{L}(N)$.

> [!quote] Dimostrazione [Sipser 1.39] - Subset Construction
> Sia $M$ così definito:
>
> -   $Q_M=\mathcal{P}(Q_N)$, [[006 Insieme delle Parti|Insieme delle Parti]]
> -   $q_M=E(q_N)\quad \textcolor{#00C575}{(\star)}$, in questo modo simulo che la computazione inizia da tutti gli stati raggiungibili da $q_N$ usando $\epsilon$-transizioni
> -   $F_M=\{R\in Q_M\ |\ R\cap F_N\neq\emptyset\}$
> -   $\delta_M:Q_M\times\Sigma\to Q_M$, dove $\forall a\in\Sigma, \forall R\in Q_M$, $$\delta_M(R,a)=E\bigg(\bigcup_{p\in R} \delta_N(p,a)\bigg)\quad \textcolor{#00C575}{(\star\star)}$$ In questo modo raggruppo gli stati e tengo in considerazione le $\epsilon$-transizioni
>
> Dobbiamo dimostrare che $\mathcal{L}(M)=\mathcal{L}(N)$, ovvero che $\forall w\in \Sigma^*$ si ha $$\hat\delta_M(q_M, w)=\hat\delta_N(q_N,w)$$
> Procediamo per induzione su $|w|$. Da questo dedurremo che $w\in \mathcal{L}(M)\Leftrightarrow w\in \mathcal{L}(N)$.
>
> -   **Passo Base**: Sia $w=\epsilon$.
>     $$\color{#61AFEF}\hat\delta_M(q_M, \epsilon)= \color{#FF6611}\text{[passo base della definizione ricorsiva di } \hat\delta \text{ nel DFA]}$$ 
>     $$\color{#61AFEF}=q_M= \color{#FF6611}\text{[per la costruzione }\textcolor{#00C575}{(\star)}\text{]}$$ 
>     $$\color{#61AFEF}=E(q_N) = \color{#FF6611}\text{[passo base della definizione ricorsiva di }\hat\delta\text{ nell'NFA]}$$ 
>     $$\color{#61AFEF}=\hat\delta_N(q_N,\epsilon)$$
>     Il passo base è dimostrato.
>
> -   **Passo Ricorsivo**: Supponiamo che $w\neq\epsilon$, ovvero che $w=xa,x\in\Sigma^*,a\in\Sigma$.
>     Per <font color="#d0d82b">ipotesi induttiva</font>, l'affermazione è vera per $x$, essendo $0\leq|x|<|w|$. Quindi $$\color{#d0d82b}\hat\delta_M(q_M,x)=\hat\delta_N(q_N,x)$$
>     Dunque
>     $$\color{#61AFEF}\hat\delta_M(q_M,xa)= \color{#FF6611}\text{[passo ricorsivo della definizione ricorsiva di }\hat\delta \text{ nel DFA]}$$ 
>     $$\color{#61AFEF}=\delta_M(\hat\delta_M(q_M,x),a)= \color{#FF6611}\text{[per la costruzione }\textcolor{#00C575}{(\star\star)}\text{, essendo }\hat\delta_M(q_M,x)\in Q_M\text{, ossia uno stato del DFA]}$$
>     $$\color{#61AFEF}=E\bigg(\bigcup_{p\in\hat\delta_M(q_M,x)}\delta_N(p,a)\bigg)=\color{#d0d82b}\text{[Ipotesi Induttiva]}$$
>     $$\color{#61AFEF}=E\bigg(\bigcup_{p\in\hat\delta_N(q_N,x)}\delta_N(p,a)\bigg)=\color{#FF6611}\text{[definizione di }\hat\delta\text{ nell'NFA]}$$
>     $$\color{#61AFEF}=\hat\delta_N(q_N,xa)$$
>     Avendo provato il passo induttivo, l'affermazione è vera $\forall w\in \Sigma^*$ per il [[046 Principio di Induzione Matematica|Principio di Induzione]].

Esistono due tipi di algoritmi per costruire un DFA a partire da un NFA, negli esercizi si usano entrambi in una particolare combinazione:

-   **Subset Construction** - Costruzione _completa_ del DFA seguendo il teorema dimostrato qui sopra. Porta alla costruzione di DFA _con tutti gli stati possibili_, quindi _molto complessi_.
-   **Lazy Construction** - Costruzione _parziale_ del DFA mostrando _solo_ gli stati _raggiungibili dal nodo iniziale_. Porta alla costruzione di DFA _minimizzati_. Questo algoritmo permette di limitare la rappresentazione del DFA, perché questi stati saranno gli unici ad essere considerati per _definire il linguaggio accettato_.
    Si suppone che si abbia già trovato lo stato iniziale $q_M$ del DFA attraverso la epsilon-chiusura dello stato iniziale $q_N$ dell'NFA, ovvero $E(q_N)$.
    1.  Inserisci lo stato iniziale $q_M$ del DFA nella lista degli stati del DFA;
    2.  Disegna lo stato iniziale $q_M$ nel disegno del DFA;
    3.  🔁 Per ogni stato $R$ nella lista degli stati del DFA:
        -   🔁 Per ogni carattere $a$ nell'alfabeto $\Sigma$:
            -   Calcola lo stato di destinazione per il simbolo $a$ partendo dallo stato $R$ usando la funzione di transizione dell'NFA, ovvero $$V=\delta_M(R,a)=E\bigg(\bigcup_{p\in R} \delta_N(p,a)\bigg)\quad \textcolor{#00C575}{(\star\star)}$$
            -   Se $V$ contiene uno stato finale dell'NFA:
                -   Imposta $V$ a stato accettante.
            -   Se $V$ non si trova nella lista degli stati del DFA:
                -   Aggiungi lo stato alla lista;
                -   Disegna lo stato nel disegno del DFA;
                -   Collega nel disegno del DFA lo stato di partenza a quello di destinazione attraverso una transazione con una etichetta del carattere $a$.
            -   Se invece esiste già nella lista:
                -   Controlla se lo stato di partenza è uguale a quello di destinazione:
                    -   Se lo è, aggiungi nel disegno del DFA un cappio sullo stato di partenza con una etichetta del carattere $a$.
                -   Se invece sono diversi:
                    -   Collega nel disegno del DFA lo stato di partenza a quello di destinazione attraverso una transazione con una etichetta del carattere $a$.
    4.  È stato creato il disegno del DFA.

> [!example]- <font color="orange">Esempio (PDF)</font>
> ![[9_1 - Esempio Teorema 1_39.pdf]]

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
