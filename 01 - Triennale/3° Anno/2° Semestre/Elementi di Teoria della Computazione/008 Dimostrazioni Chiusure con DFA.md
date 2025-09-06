---
aliases:
  - Dimostrazione chiusura linguaggi unione (DFA)
  - Dimostrazione chiusura linguaggi intersezione
  - Dimostrazione chiusura linguaggi complemento
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Automi Deterministici
cssclasses:
  - 
---

Qui vengono riportate tutte le dimostrazioni **possibili con i [[004 Automi Finiti Deterministici|DFA]]** per le [[Chiusura di un_opearzione|chiusure]] delle operazioni su [[007 Linguaggio Regolare|linguaggi regolari]].

Per vedere il resto andare su [[013 Dimostrazioni Chiusure con NFA]].


## Unione

> [!quote]+ Dimostrazione per l'unione [Sipser - Teorema 1.25]
>
> > La classe dei linguaggi regolari è chiusa rispetto all'operazione di [[008 Unione tra Insiemi|unione]].
>
> Ovvero, dati due linguaggi regolari $A_1$ e $A_2$, dobbiamo dimostrare che $$A_1 \text{ e } A_2 \text{ regolari } \Rightarrow A_1\cup A_2 \text{ regolare}$$
>
> > [!error] In generale NON è vero che se l'unione di due linguaggi è regolare allora i due linguaggi sono regolari
> > $$A_1\cup A_2 \text{ regolare}\not\Rightarrow A_1 \text{ e } A_2 \text{ regolari } $$
>
> #### Idea
>
> Poiché $A_1$ e $A_2$ sono regolari sappiamo che:
>
> -   $\exists M_1 \text{ t.c. } \mathcal{L}(M_1)=A_1$, ovvero che esiste un automa $M_1$ che riconosce come linguaggio $A_1$
> -   $\exists M_2 \text{ t.c. } \mathcal{L}(M_2)=A_2$, ovvero che esiste un automa $M_2$ che riconosce come linguaggio $A_2$
>
> Per dimostrare che la loro unione è ancora regolare, bisogna costruire $M \text{ t.c. } \mathcal{L}(M)=A_1\cup A_2$
>
> Questa è una prova costruttiva. Costruiamo $M$ da $M_1$ ed $M_2$. Per riconoscere il linguaggio unione, la macchina $M$ deve accettare il suo input esattamente quando $M_1$ o $M_2$ lo accetterebbero (per via della definizione dell'unione). Opera _simulando_ sia $M_1$ che $M_2$, _accettando se l'una o l'altra delle simulazioni accetta_.
>
> Quando i simboli dell'input sono stati letti e usati per simulare $M_1$, non si può "riavvolgere il nastro di input" per provare la simulazione per $M_2$ su un input precedente. Quindi $M$ deve ricordare lo stato in cui ciascuna macchina sarebbe se avesse letto l'input fino a quel punto. Ovvero, bisogna _ricordare una coppia di stati_. Dove le transizioni di $M$ vanno da coppia a coppia, aggiornando lo stato corrente sia per la componente in $M_1$ che per quella in $M_2$.
>
> Questa coppia può essere rappresentata attraverso il diagramma di stato, dove il numero di stati in $M$ è il prodotto tra il numero di stati in $M_1$ per quello di $M_2$:
>
> ![[Pasted image 20240311091034.png|700]]
>
> Gli stati accettanti di $M$ sono quelle coppie tali che $M_1$ oppure $M_2$ è in uno stato accettante.
>
> ![[Pasted image 20240311091448.png|700]]
>
> #### Dimostrazione
>
> Supponiamo che
> $$M_1 \text{ riconosca } A_1, \text{ dove } M_1=(Q_1,\Sigma,\delta_1,q_{01},F_1)$$ 
> $$M_2 \text{ riconosca } A_2, \text{ dove } M_2=(Q_2,\Sigma,\delta_2,q_{02},F_2)$$
> Costruiamo $M$ che riconosce $A_1\cup A_2$, dove $M=(Q,\Sigma,\delta, q_0,F)$:
>
> 1.  $\color{#CC241D}Q$ è il [[012 Prodotto Cartesiano tra Insiemi|prodotto cartesiano]] degli insiemi $Q_1$ e $Q_2$ ed è denotato con $Q_1\times Q_2$. È _l'insieme di tutte le coppie di stati_.
>     $$\color{#CC241D}Q=\{(r_1,r_2)\ |\ r_1\in Q_1 \land r_2\in Q_2\}$$
> 2.  $\color{#CC241D}\Sigma$, l'alfabeto, è lo stesso di $M_1$ ed $M_2$. Infatti, per semplicità si assume che abbiano lo stesso alfabeto per i simboli di input.
> 3.  $\color{#CC241D}\delta$, la [[004 Automi Finiti Deterministici#Funzione di transizione|Funzione di Transizione]] è definita come segue, per ogni $(r_1,r_2)\in Q$ e ogni $a\in\Sigma$:
>     $$\color{#CC241D}\delta((r_1,r_2), a)=(\delta_1(r_1, a), \delta_2(r_2,a))$$
>     Quindi $\delta$ riceve uno stato di $M$ (una coppia di stati), con un simbolo di input, e restituisce lo stato successivo di $M$.
>     $$\hat\delta((q_{01}, q_{02}), w)=(r_1,r_2)$$
> 4.  $\color{#CC241D}q_0$ è la coppia $\color{#CC241D}(q_{01},q_{02})$
> 5.  $\color{#CC241D}F$ è l'insieme delle coppie in cui l'uno o l'altro elemento è lo stato accettante di $M_1$ o $M_2$, quindi è
>     $$\color{#CC241D}F=\{(r_1,r_2)\ |\ r_1\in F_1 \lor r_2\in F_2\}$$
>
> Questo conclude la costruzione dell'automa finito $M$ che riconosce come linguaggio $A_1\cup A_2$. La correttezza è evidente dalla strategia descritta nell'idea della prova, ma bisognerebbe provare formalmente che
> $$\hat\delta(q_0, w)\in F \Leftrightarrow \hat\delta_1(q_{01},w)\in F_1 \lor \hat\delta_2(q_{02},w)\in F_2$$
> Ovvero che se viene raggiunto uno stato finale a partire dallo stato iniziale $q_0=(q_{01},q_{02})$ di $M$, allora:
>
> -   O viene raggiunto uno stato finale in $F_1$ su $M_1$ a partire dallo stato iniziale $q_{01}$
> -   Oppure viene raggiunto uno stato finale in $F_2$ su $M_2$ a partire dallo stato iniziale $q_{02}$

È possibile dimostrare questo anche attraverso un NFA ([[013 Dimostrazioni Chiusure con NFA#Unione|qui]]).


## Intersezione

> [!quote]+ Dimostrazione per l'intersezione
>
> > La classe dei linguaggi regolari è chiusa rispetto all'operazione di [[009 Intersezione tra Insiemi|intersezione]].
>
> > [!error] In generale NON è vero che se l'intersezione di due linguaggi è regolare allora i due linguaggi sono regolari
> > $$A_1\cap A_2 \text{ regolare}\not\Rightarrow A_1 \text{ e } A_2 \text{ regolari } $$
> > Questo perché se consideriamo $A_1=ww$, con $w\in\{a,b\}^*$ e $A_2=\{a\}^*$, abbiamo che $A_1\notin REG$  e $A_2\notin REG$. L'intersezione $A_1\cap A_2=ww \text{ t.c.} w=\{a,b\}^*\cap\{a\}^*=\{a\}^*=a^{2n}$ con $n\geq 0$ è regolare.
>
> La dimostrazione è simile a quella per l'unione, ma quando si trova l'insieme di stati accettanti $F$ di $M$, si vede se
> $$F=\{(r_1,r_2)\ |\ r_1\in F_1 \textcolor{#CC241D}{\land} r_2\in F_2\}$$

---

> [!example]- <font color="orange">Esempio unione e intersezione</font>
> Costruire un DFA che riconosce le parole che hanno un numero **pari di $a$** **_e/o_** un numero **dispari di $b$**.
>
> ![[Pasted image 20240320113430.png]]


## Complemento

> [!quote]+ Dimostrazione per il complemento [HUM - Teorema 4.5]
>
> > Se $L$ è un linguaggio regolare sull'alfabeto $\Sigma$, allora anche il [[013 Complemento di un Insieme|complemento]] $\overline{L}=\Sigma^*-L$ è un linguaggio regolare.
>
> Creo una “copia” di $M_1$ e rendo finali gli stati non finali, e vice-versa.
>
> ![[Pasted image 20240320115221.png|450]]
>
> Da notare che gli stati rifiutati nella "copia" sono gli stati rifiutati di $\Sigma^*$ e non di $L$.


## Differenza

> [!quote]+ Dimostrazione per l'intersezione
>
> > La classe dei linguaggi regolari è chiusa rispetto all'operazione di [[010 Differenza tra Insiemi|differenza]].
>
> La dimostrazione utilizza quella di intersezione e quella di complemento dato che per la definizione di differenza si ha $$L-M = L\cap \overline{M}$$


## Motivazione Fallimento per la Concatenazione

Con i DFA non è possibile avere la dimostrazione. Andare [[013 Dimostrazioni Chiusure con NFA#Concatenazione|qui]] per la vera dimostrazione.

> [!error]+ Dimostrazione per la concatenazione [Sipser - Teorema 1.26]
> La classe dei linguaggi regolari è chiusa rispetto all'operazione di [[002 Operazioni su Linguaggi#Concatenazione (o prodotto) di Linguaggi|concatenazione]].
>
> In altre parole, se $A_1$ e $A_2$ sono linguaggi regolari, allora lo è anche $A_1\circ A_2$.
>
> Per provare questo teorema, utilizziamo qualcosa di simile alla prova del caso dell'unione. Come prima, possiamo iniziare con gli automi finiti $M_1$ ed $M_2$ che riconoscono i linguaggi regolari $A_1$ e $A_2$.
>
> Ma ora, invece di costruire l'automa $M$ che accetta il suo input se $M_1$ oppure $M_2$ lo accetta, esso deve _accettare se il suo input può essere diviso in due parti_, tali che:
>
> -   $M_1$ accetta la prima parte
> -   $M_2$ accetta la seconda parte.
>
> Il problema è che $M$ non sa _dove dividere_ il suo input (cioè, dove finisce la prima parte e inizia la seconda). Per risolvere questo problema, si deve introdurre un nuovo tipo di automa, quello [[009 Automi Finiti Non Deterministici|non deterministico]]. Questa separazione avverrà attraverso l'utilizzo della stringa vuota $\color{#61AFEF}\epsilon$.

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
