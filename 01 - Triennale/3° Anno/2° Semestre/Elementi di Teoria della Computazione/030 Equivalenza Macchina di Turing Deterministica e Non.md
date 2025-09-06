---
aliases:
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Varianti delle Macchine di Turing
cssclasses:
    - 
---

> Per ogni Macchina di Turing [[029 Macchina di Turing Non Deterministica|Non Deterministica]] $N$ esiste una Macchina di Turing [[018 Macchine di Turing|Deterministica]] $D$ **equivalente** ad $N$, cioè tale che entrambe le macchine [[023 Linguaggio Turing-Riconoscibile|riconoscano lo stesso linguaggio]].
> $$\mathcal{L}(N)=\mathcal{L}(D)$$

> [!tip]- Macchine equivalenti
> ![[025 Varianti delle Macchine di Turing#^7ce7c4]]


## Dimostrazione

> Data una macchina di Turing non deterministica $N=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$, la macchina di Turing $D$ che simula $N$ ha [[026 Macchina di Turing Multinastro|tre nastri]].
> Sappiamo che esiste una macchina di Turing deterministica a un nastro [[027 Equivalenza tra Multinastro e Singolo Nastro|equivalente]] ad $D$ e quindi a $N$.

### Idea

L'idea di base consiste nel far **simulare** a $D$ _tutte le possibili scelte_ che può fare $N$ durante la sua [[029 Macchina di Turing Non Deterministica#Computazione|computazione non deterministica]].

Quindi, per ogni input $w$, $D$ esplora l'[[029 Macchina di Turing Non Deterministica#Albero delle computazioni|albero delle computazioni]] di $N$ su $w$.

La macchina $D$ esplora questo albero alla ricerca di una [[021 Configurazione di una Macchina di Turing#^f0bd9d|Configurazione di Accettazione]]:

-   Se $D$ trova lo stato di accettazione _su uno qualsiasi dei rami_ dell'albero, allora $D$ accetta. Questo assicura che $\mathcal{L}(D) = \mathcal{L}(N)$.
-   Altrimenti, la simulazione di $D$ _non terminerà_.

> [!failure]- Strategia sbagliata
> Far esplorare a $D$ l'albero utilizzando la [[015 Depth First Search|Depth First Search]] _non è_ una buona idea, questo perché la vista rischierebbe di rimanere "incastrata" in un cammino corrispondente a una _computazione che non termina_, escludendo un'eventuale accettazione.

> [!success]+ Strategia da usare
> Progettiamo $D$ in modo da esplorare l'albero utilizzando la [[014 Breadth First Search|Breadth First Search]] (_per livelli_). Questo modo garantisce che $D$ visita ogni nodo dell'albero finché non incontra una configurazione di accettazione.

Rappresenteremo le computazioni di $N$ su una stringa $w$ (ovvero i cammini nell'albero delle computazioni di $N$ su $w$) _attraverso una stringa_.

### Rappresentazione delle Computazioni

La macchina $D$ impiega i suoi tre nastri in modo specifico:

-   Il **primo nastro** contiene sempre la stringa di input e _non viene mai modificato_.
-   Sul **secondo nastro** viene eseguita la simulazione di $N$. Mantiene una copia del nastro di $N$ _corrispondente a qualche diramazione_ dell'albero delle computazioni.
-   Sul **terzo nastro** vengono generate le _codifiche delle possibili computazioni_ di $N$ con input $w$. Tiene traccia della _posizione_ di $D$ nell'albero delle computazioni di $N$, rappresentata _mediante una stringa_.

![[Pasted image 20240506141704.png|700]]

### Rappresentazione degli indirizzi sul nastro 3

Ogni nodo dell'albero può aver al massimo $b$ figli, dove $b$ è la **dimensione del più grande insieme di scelte possibili** date dalla [[029 Macchina di Turing Non Deterministica#Definizione|funzione di transizione]] di $N$ _su ogni stato e ogni carattere_, ovvero $$b=\max(\{|\delta(q,\sigma)|:q\in Q,\sigma\in\Sigma\})$$
Ad ogni nodo della struttura assegniamo un **indirizzo**, ovvero una stringa sull'alfabeto $\Gamma_b=\{1,\dots,b\}$.
Ogni simbolo della stringa ci dice _quale deve essere la scelta successiva_ per trovare uno _specifico nodo_, durante la simulazione di un passo in una ramificazione di una computazione non deterministica in $N$.

> [!note] La stringa che rappresenta un cammino è anche l'indirizzo del nodo finale del cammino

Ovvero, per ogni coppia stato-simbolo $(q,\sigma)$ esistono al più $b$ scelte, a ognuna delle quali associamo un simbolo diverso in $\Gamma_b$. Quindi per ogni configurazione $u\ q\sigma\ v$ esistono al più $b$ successive configurazioni a ognuna delle quali risulta associato un diverso simbolo in $\Gamma_b$.

Ad ogni computazione è associata una stringa su $\Gamma_b$ e, viceversa, ad ogni stringa $\Gamma_b$ è associata al più di una computazione. Quindi _non è sempre vero_ che una stringa rappresenta una computazione.

> [!example]- <font color="orange">Esempio</font>
> Per la seguente Macchina di Turing non deterministica, con input `bab`
>
> ![[Pasted image 20240506122446.png|400]]
>
> abbiamo il seguente albero delle computazioni, dove ogni nodo viene etichettato con il proprio indirizzo (numero da 1 a 3).
>
> ![[Pasted image 20240506142907.png|500]]
>
> La computazione che mostra che `bab` è accettata è data dall'indirizzo del nodo foglia $babb\ q_{accept}$, ovvero `2131`. Questo è il percorso che segue la computazione.
>
> ![[Immagine 2024-05-06 143307.png|500]]

-   La stringa vuota corrisponde alla configurazione iniziale, ovvero all'indirizzo della radice dell'albero.
-   Una visita per livelli dell'albero corrisponde alla lista delle stringhe su $\Gamma_b$ in [[028 Ordine Radix|ordine radix]].
-   La macchina $D$ che simula $N$ utilizzerà come "sottoprogramma" una macchina di Turing che genera la successiva stringa in base b in ordine radix.

### Descrizione di $D$

1. Inizialmente il nastro 1 contiene l'input $\color{#61AFEF}w$ ed i nastri 2 e 3 sono _vuoti_ (contengono _solo $\sqcup$_). ^826204
2. $D$ _copia il nastro 1 sul nastro 2_ ed inizializza la stringa sul nastro 3 ad $\color{#61AFEF}\epsilon$. ^5a9818
3. Utilizza il nastro 2 per simulare $N$ con input $w$ _su una ramificazione_ della sua computazione non deterministica corrispondente alla stringa sul nastro 3, cioè _riproduce la successione di scelte_ del corrispondente cammino (se tale stringa corrisponde a una computazione).
   Prima di ogni passo di $N$, consulta il simbolo successivo sul nastro 3 per determinare quale scelta fare tra quelle consentite dalla funzione di transizione di $N$. ^832900
    - Se incontra una _configurazione di accettazione_, allora _accetta l'input_. Pertanto, $D$ accetta $w$ se e solo se $N$ accetta $w$, quindi $\color{#CC241D}\mathcal{L}(D)=\mathcal{L}(N)$.
    - Altrimenti, se _non rimangono più simboli_ sul nastro 3 o se questa scelta non deterministica _non è valida_ o se si incontra una _configurazione di rifiuto_, interrompe questo cammino andando alla fase 4.
4. Sostituisce la stringa sul nastro 3 con la stringa successiva rispetto all'ordine radix. Simula la ramificazione successiva della computazione di $N$ andando alla fase 2.

> [!example]- <font color="orange">Esempi</font>
> ![[Pasted image 20240506153839.png]]
>
> $\Gamma_b=\{1,2\}$
>
> $N_1=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ è tale che $\delta(q_1,a)=\{(q_{reject}, a,R),(q_1,a,R)\}$. Supponiamo di associare:
>
> -   1 alla scelta $(q_{reject}, a,R)$
> -   2 alla scelta $(q_1,a,R)$
>
> Poniamo $D=(Q',\Sigma, \Gamma',\delta',q_0',q_{accept}, q_{reject}')$.
>
> Siano $p_1$ e $p_{reject}$ gli stati che corrispondono rispettivamente a $q_1$ e $q_{reject}$ nella simulazione di $N_1$, dove può anche accadere che $p_1=q_1$.
> Allora $\delta'(p_1,\sigma,a,1)=(p_{reject},\sigma,a,1,S,R,R)$, per ogni carattere $\sigma$.
> Dove:
>
> -   $p_1, p_{reject}$ sono gli stati
> -   $\sigma$ è l'input sul primo nastro
> -   $a$ è il carattere letto sul secondo nastro
> -   $1$ è l'indirizzo letto sul terzo nastro
> -   La direzione 1 $(S)$ si riferisce al primo nastro, che deve rimanere sempre fermo
> -   La direzione 2 $(R)$ si riferisce al secondo nastro, che in questo caso si sposta a destra
> -   La direzione 3 $(R)$ si riferisce al terzo nastro, che in questo caso si sposta a destra
>
> Nello stato $p_{reject}$, $D$ va al passo 4.
>
> ---
>
> ![[Pasted image 20240506122446.png|400]]
>
> $\Gamma_b=\{1,2,3\}$
>
> $N_2=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ è tale che $\delta(q_0,b)=\{(q_0,b,R), (q_1,b,R), (q_{reject},b,R)\}$. Supponiamo di associare:
>
> -   1 alla scelta $(q_{reject},b,R)$
> -   2 alla scelta $(q_0,b,R)$
> -   3 alla scelta $(q_1,b,R)$
>
> Poniamo $D=(Q',\Sigma, \Gamma',\delta',q_0',q_{accept}, q_{reject}')$.
>
> Siano $p_0,p_1,p_{reject}$ gli stati che corrispondono rispettivamente a $q_0,q_1,q_{reject}$ nella simulazione di $N_2$, dove può anche accadere che $p_0=q_0$ e $p_1=q_1$.
> Allora, per ogni carattere $\sigma$:
>
> -   $\delta'(p_0,\sigma,b,2)=(p_0,\sigma,b,2,S,R,R)$
> -   $\delta'(p_0,\sigma,b,3)=(p_1,\sigma,b,3,S,R,R)$
> -   $\delta'(p_0,\sigma,b,1)=(p_{reject},\sigma,b,1,S,R,R)$
>
> Nello stato $p_{reject}$, $D$ va al passo 4.


## Linguaggio Turing-Riconoscibile per le Non Deterministiche

![[023 Linguaggio Turing-Riconoscibile#^5b96ec|Linguaggio accettato da una MdT]]

Questo è vero anche per le Macchina di Turing Non Deterministiche.

> [!quote] Dimostrazione
> Se $L$ è il linguaggio $\mathcal{L}(N)$ riconosciuto da una macchina di Turing Non Deterministica $N$, per il teorema precedente esiste una Macchina di Turing Deterministica $D$ tale che $\mathcal{L}(D)=\mathcal{L}(N)=L$.
>
> Quindi $L$ è Turing-Riconoscibile.
>
> Viceversa, se $L$ è Turing-Riconoscibile, allora esiste una macchina di Turing Deterministica $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ che lo riconosce, cioè tale che $\mathcal{L}(M)=L$.
> È facile vedere che la macchina di Turing non deterministica $M'=(Q,\Sigma, \Gamma,\delta',q_0,q_{accept}, q_{reject})$, dove $\delta'(q,\gamma)=\{(q,\gamma)\}$, per ogni $q\in Q-\{q_{accept},q_{reject}\}$ e $\gamma\in\Gamma$, è tale che $\mathcal{L}(M')=\mathcal{L}(M)=L$.

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
