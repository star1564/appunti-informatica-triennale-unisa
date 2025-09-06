---
aliases:
    - Macchina di Turing Universale
    - ATM
    - Problema indecidibile
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Indecidibilità
cssclasses:
    - 
---

## Definizione Problema Indecidibile

> Un [[001 Logica Proposizionale#^proposizione|problema decisionale]] $P$ si dice **Indecidibile** se il linguaggio $L$ di tutte le [[033 Istanze di un Problema di Decisione|istanze]] affermative di $P$ _non è decidibile_, ovvero che _non esiste alcuna Macchina di Turing che [[024 Macchine Decisionali#Linguaggio Deciso|decide]] quel linguaggio_.

Un linguaggio indecidibile può anche essere un linguaggio parzialmente decidibile o qualcosa di diverso, ma non decidibile.

> [!note] Un linguaggio indecidibile è [[Chiusura di un_opearzione|chiuso]] rispetto al complemento
> Quindi se un linguaggio è indecidibile, lo sarà anche il suo complemento e viceversa.


## Dimostrazione ($A_{TM}$)

Il [[038 Metodo della Diagonalizzazione|metodo della diagonalizzazione]] dimostra l'esistenza di linguaggi [[040 Linguaggi Non Turing-Riconoscibili|Non Turing-Riconoscibili]], ma la prova _non era costruttiva_.

Verrà presentato _un linguaggio specifico_, collegato a un problema di decisione indecidibile, utilizzando la _diagonalizzazione e l'autoreferenzialità_.

> [!info]- Paradosso di Russel
> ![[Paradosso di Russel]]

> [!quote] Teorema 4.11 [Sipser]
> Il seguente linguaggio $A_{TM}$ è _[[023 Linguaggio Turing-Riconoscibile#^5b96ec|Turing-Riconoscibile]]_, ma _Non è decidibile_ (è **Indecidibile**)
> $$A_{TM}=\{\langle M,w \rangle\ |\ M\text{ è una Macchina di Turing e }M\text{ accetta la parola }w\}$$

^19746f

### Macchina di Turing Universale

> Una **Macchina di Turing Universale** $U$ riceve in input una [[036 Codifiche|codifica]] $\langle M,w \rangle$ e _simula la computazione_ della Macchina di Turing $M$ sull'input stringa $w$.

> [!note] Descrizione di $U$
> Sull'input $\langle M,w \rangle$, dove $M$ è una Macchina di Turing e $w$ è una stringa:
>
> 1. Simula $M$ sull'input $w$
> 2. ➜ Se $M$ accetta $w$, allora accetta l'input $\langle M,w \rangle$
>    ➜ Se $M$ rifiuta $w$, allora rifiuta l'input $\langle M,w \rangle$

Inoltre:

-   Se la computazione di $M$ non termina, anche la computazione di $U$ _non terminerà_.
-   $U$ rifiuta ogni stringa _che non sia_ della forma $\langle M,w \rangle$

#### Simulazione con tre nastri

Si può pensare alla Macchina di Turing Universale come una macchina a [[026 Macchina di Turing Multinastro|3 nastri]]. Infatti:

1. Usa il _primo nastro_ per simulare la computazione di $M$
2. Lascia sul _secondo nastro_ la codifica di $M$
3. Usa il _terzo nastro_ per la codifica dello stato corrente di $M$, all'inizio contiene la codifica dello stato iniziale di $M$

$U$ individua l'istruzione corrente sul secondo nastro, usando il contenuto del terzo nastro ed il simbolo corrente (codificato) sul primo nastro, quindi decodifica l'istruzione e la esegue.

### Dimostrazione che $A_{TM}$ è Turing-Riconoscibile

> La Macchina di Turing Universale $U$ **riconosce** $A_{TM}$.

Infatti, $U$ accetta una stringa $y$ _se e solo se è della forma $\langle M,w \rangle$_, il che è un elemento di $A_{TM}$.
Ne segue che $\color{#CC241D}\mathcal{L}(U)=A_{TM}$.

> [!note]+ Nota
> Si ha che $U$ **non decide** $A_{TM}$, perché $U$ può anche non terminare su $\langle M,w \rangle$ se $M$ non termina su $w$.

### Dimostrazione che $A_{TM}$ non è decidibile

> Il linguaggio $A_{TM}$ **non è [[024 Macchine Decisionali#Linguaggio Decidibile|decidibile]]**.

![[024 Macchine Decisionali#^8e9c30]]

Assumiamo per assurdo che $A_{TM}$ sia decidibile.

#### Decisore H

Supponiamo quindi che esista un [[024 Macchine Decisionali#Definizione|decisore]] $H$ che riconosca $A_{TM}$.
Dove $\color{#FF6611}H(\langle M,w \rangle)$:

-   <font color="#FF6611">Accetta</font> le stringhe di $A_{TM}$, quindi se $\langle M,w \rangle\in A_{TM}$
-   <font color="#FF6611">Rifiuta</font> le stringhe che
    -   non sono in $A_{TM}$, quindi se $\langle M,w \rangle\not\in A_{TM}$
    -   non sono della forma $\langle M,w \rangle$

Quindi:
$$\textcolor{#FF6611}{H(\langle M,w \rangle)}=\begin{cases} \text{accetta\quad se }M\text{ accetta }w \\ \text{rifiuta\quad se }M\text{ non accetta }w \end{cases}$$

#### Decisore D

Ora costruiamo una nuova Macchina di Turing $D$ avente $H$ come _sottoprogramma_. Questa nuova macchina chiama $H$ per determinare cosa fa $M$ quando l'input è _la sua stessa codifica_ $\langle M \rangle$.

Dove $\color{#00C575}H(\langle M,\langle M \rangle \rangle)$:

-   <font color="#00C575">Accetta</font> se $M$ accetta $\langle M \rangle$
-   <font color="#00C575">Rifiuta</font> se $M$ non accetta $\langle M \rangle$

Una volta che $D$ ha determinato questa informazione, essa _fa il contrario_.

> [!note] Descrizione di $D$
> Su input $\langle M \rangle$, dove $M$ è una Macchina di Turing:
>
> 1. Simula $\color{#00C575}H$ quando $\color{#00C575}H$ riceve in input $\langle M,\langle M \rangle \rangle$
> 2. Dà in output l'opposto di ciò che $H$ da in output. Ovvero:
>    ➜ Accetta se $\color{#00C575}H(\langle M,\langle M \rangle \rangle)$ rifiuta
>    ➜ Rifiuta se $\color{#00C575}H(\langle M,\langle M \rangle \rangle)$ accetta

Quindi:
$$\textcolor{#53ccc5}{D(\langle M\rangle)}=\begin{cases} \text{accetta\quad se }M\text{ non accetta }\langle M \rangle \\ \text{rifiuta\quad se }M\text{ accetta }\langle M \rangle \end{cases}$$

Per via del fatto che $D$ può essere facilmente costruita a partire da $H$, abbiamo che se $H$ esiste, esisterà anche $D$.

Se diamo in input a $D$ la sua stessa codifica $\langle D \rangle$, abbiamo che:
$$\textcolor{#53ccc5}{D(\langle D\rangle)}=\begin{cases} \text{accetta\quad se }D\text{ non accetta }\langle D \rangle \\ \text{rifiuta\quad se }D\text{ accetta }\langle D \rangle \end{cases}$$
Indipendentemente da ciò che $D$ fa, essa è costretta a fare il contrario, il che è ovviamente una contraddizione. Quindi, entrambe le macchine $D$ e $H$ **non possono esistere**. Si è trovato l'assurdo.

### Utilizzo della diagonalizzazione

Nella prova precedente è stato utilizzato il metodo della diagonalizzazione. Diventa evidente quando si esaminano le tavole del comportamento di $H$ e $D$. In queste tavole indicizziamo le righe con tutte le Macchine di Turing $M_1,M_2,\dots$ e indicizziamo le colonne con le codifiche $\langle M_1 \rangle,\langle M_2 \rangle,\dots$ di tali macchine.

Le entry dicono se la macchina di una determinata riga accetta l'input di una data colonna. Ovvero, l'entry $i,j$ è il valore di $H$ su input $\langle M_i,\langle M_j \rangle \rangle$:

-   La entry $i,j$ è $accetta$ se $M_i$ accetta $\langle M_j \rangle$
-   La entry $i,j$ è $rifiuta$ se $M_i$ rifiuta $\langle M_j \rangle$

![[Pasted image 20240520121244.png]]

Possiamo aggiungere $D$ nella tabella, infatti deve comparire nella lista $M_1,M_2,\dots$ di tutte le Macchine di Turing. Da notare che $D$ calcola il contrario degli elementi sulla diagonale.

![[Pasted image 20240520121437.png]]

La contraddizione si ottiene in corrispondenza del punto interrogativo, dove l'entry dovrebbe essere _l'opposto di se stessa_.

---

![[043 Linguaggi Co-Turing-Riconoscibili#$ overline{A_{TM}}$ Non è Turing-Riconoscibile (4.23)]]

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

-   [GeeksForGeeks](https://www.geeksforgeeks.org/decidability-and-undecidability-in-toc/)
