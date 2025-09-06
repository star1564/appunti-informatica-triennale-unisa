---
aliases:
  - Pigeonhole Principle
  - Principio della Piccionaia
  - Provare la Non Regolarità di un Linguaggio
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Espressioni Regolari
cssclasses:
  - 
---
>Il **Pumping Lemma** (PL) è una tecnica per provare la [[016 Linguaggio Non Regolare|non regolarità di un linguaggio]]. Questo teorema dice che tutti i [[007 Linguaggio Regolare|linguaggi regolari]] hanno una *proprietà speciale*. Se possiamo dimostrare che un linguaggio *non ha questa proprietà*, allora il linguaggio *non è regolare*.

La proprietà afferma che tutte le stringhe in un linguaggio *avranno una sezione che può essere ripetuta* (pumped) un qualsiasi numero di volte ottenendo una stringa che *appartiene ancora al linguaggio*. 

Questo accade se la lunghezza di *tutte le stringhe* del linguaggio sono almeno di una certa lunghezza, chiamata la **lunghezza del pumping**.

![[Immagine 2024-03-28 095800.png|600]]

> [!quote] Pumping Lemma
> Se $A$ è un linguaggio regolare, allora esiste un numero $p>0$ (la lunghezza del pumping) tale che se $s$ è una qualsiasi stringa in $A$ di lunghezza almeno $p$ (ovvero che $|s|\geq p$), allora $s$ può essere divisa in tre parti, $s=\textcolor{#e86162}x\textcolor{#61AFEF}y\textcolor{#fdaa39}z$, soddisfacenti le seguenti condizioni:
> 1. per ogni $i\geq0$, $\textcolor{#e86162}x\textcolor{#61AFEF}{y^i}\textcolor{#fdaa39}z\in A$
> 2. $|\textcolor{#61AFEF}y|>0$, ovvero che $\textcolor{#61AFEF}y\neq\epsilon$
> 3. $|\textcolor{#e86162}x\textcolor{#61AFEF}y|\leq p$

1. Il primo punto dice che per ogni stringa nel linguaggio regolare esiste una parte che può essere ripetuta (pumped) un qualsiasi numero di volte. Questa parte è chiamata $\color{#61AFEF}y$.
2. Il secondo punto dice che $\color{#61AFEF}y$ non deve essere una stringa vuota. Quindi solo $\color{#e86162}x$ e $\color{#fdaa39}z$ possono essere stringhe vuote.
	- Questo **NON** significa che $|\textcolor{#61AFEF}{y^i}|>0$, infatti, può anche essere vero che $|\textcolor{#61AFEF}{y^i}|=0$
3. Il terzo punto dice che le parti $\color{#e86162}x$ e $\color{#61AFEF}y$ insieme hanno una lunghezza massima di $p$. Ovvero che:
	- La lunghezza di $\color{#61AFEF}y$ può essere al massimo $p$
	- $\color{#61AFEF}y$ si deve trovare nei primi $p$ simboli

![[Pasted image 20240328101334.png|800]]

Quindi, se un linguaggio è regolare, allora ogni stringa nel linguaggio potranno seguire queste tre condizioni.

## Dimostrazione della verità del Pumping Lemma

> [!info]+ Pigeonhole Principle
> Se abbiamo $p$ gabbiette e $m$ piccioni ($m>p$) ci sarà una gabbietta con almeno 2 piccioni.
> 
> ![[Pasted image 20240328103831.png|500]]

>Se un automa con $p$ stati accetta una stringa di lunghezza $n$ ($n\geq p$), allora per ogni cammino di accettazione deve esistere *almeno uno stato che si ripete*.

Sia $q$ il primo stato che si ripete, $s=xyz$ e $p$ il numero di stati.

![[Pasted image 20240328105301.png|900]]

Possiamo notare che:
- $|xy|\leq p$ perché in $xy$ nessuno stato si ripete, ad eccezione di $q$
- La $y$ può essere considerata come una sorta di *loop*, in cui *si può o non può entrare*. Infatti:
	- Se non si entra nel loop, si andrà una sola volta su $q$
	- Se si entra nel loop, si andrà almeno due volte su $q$
- $|y|\geq 1$ perché c'é almeno una transazione nel loop
- La forma di $z$ non ci interessa

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di avere il linguaggio seguente $$A=\{\text{tutte le stringhe che finiscono in }11\}$$
>con $p=3$, ovvero il numero di stati. Avremo il seguente [[004 Automi Finiti Deterministici|DFA]]:
>
>![[Pasted image 20240328104714.png|500]]
>
>Le stringhe $011,1011,01111$ sono accettate, quindi per ognuna di queste ci deve essere almeno uno stato che si è ripetuto. In questi tre casi, lo stato $q_0$ si è ripetuto.

## Applicazione del Pumping Lemma - Provare la Non Regolarità di un Linguaggio
Per provare che un linguaggio non è regolare, si usa il pumping lemma.

>Tutti *i linguaggi regolari soddisfano il Pumping Lemma*, ovvero che le stringhe nel linguaggio possono essere "replicate". Quindi, se un linguaggio $L$ *non lo soddisfa*, ovvero che le stringhe non possono essere "replicate", allora $L$ *non è regolare*.

Quindi, per mostrare che $L$ non è regolare, mostriamo che $L$ non soddisfa il Pumping Lemma.

Questo avviene attraverso una **prova per assurdo**:
1. Si suppone che il linguaggio *$L$ è regolare*. Vale quindi il pumping lemma.
2. Si usa il pumping lemma per *ottenere la lunghezza del pumping $p>0$* tale che *tutte le stringhe* di $L$ di lunghezza maggiore o uguale a $p$ possono essere replicate.
3. *Trovare una stringa* $s$ in $L$ che ha *lunghezza maggiore o uguale a $p$* che *non può essere iterata* considerando TUTTI i possibili modi di fattorizzarla in $x,y,z$. Utilizzeremo questa stringa per contraddire l'assuzione iniziale che il linguaggio soddisfa il pumping lemma. 
4. Per ogni fattorizzazione, si *trova $i$* tale che $xy^iz$ *non appartiene* a $L$. L'esistenza di $s$ contraddirebbe il pumping lemma se $L$ fosse regolare, quindi non lo può essere.

> [!tip]- Trovare la stringa
> La stringa può essere trovata, descrivendo nel modo più piccolo possibile le stringhe del linguaggio.
> $\{a^nb^n:n\geq 0\}\to a^pb^p$
> $\{a^ib^j:i=2j,\ i,j\geq 0\}\to a^pb^{p/2}$


> [!example]+ <font color="orange">Esempio Base</font>
>Sia $L=\{a^nb^n:n\geq0\}$ un linguaggio. Vogliamo provare che $L$ non è regolare.
>
>Supponiamo per assurdo che $L$ sia regolare. Sia $p$ la lunghezza del pumping.
>
>La stringa $\color{#CC241D}s=a^pb^p$ è nel linguaggio e $|s|=2p$, quindi soddisfa le ipotesi ($|s|\geq p$) e può essere divisa in tre parti.
>
>Consideriamo i tre casi per mostrare che questo risultato è impossibile:
>1. La stringa $y$ consiste *solo di simboli uguali ad $a$*. In questo caso, la stringa ha più simboli uguali ad $a$ che simboli uguali a $b$ e quindi non è un elemento di $L$.
>   $$\textcolor{#61AFEF}{a^p}b^p\Rightarrow\textcolor{#61AFEF}{a..a..a..a..}a^pb^p\not\in L$$
>2. La stringa $y$ consiste *solo di simboli uguali a $b$*. In questo caso, la stringa ha più simboli uguali a $b$ che simboli uguali ad $a$ e quindi non è un elemento di $L$.
>   $$a^p\textcolor{#61AFEF}{b^p}\Rightarrow a^pb^p\textcolor{#61AFEF}{..b..b..b..b..}\not\in L$$
>3. La stringa $y$ consiste *sia di simboli uguali ad $a$ che di simboli uguali a $b$*. In questo caso, la stringa può avere lo stesso numero di simboli uguali ad $a$ e simboli uguali a $b$, ma essi non saranno nell'ordine corretto, con qualche $b$ prima di qualche $a$. Quindi non è un elemento di $L$, il che è una contraddizione.
>   $$\textcolor{#61AFEF}{a^pb^p}\Rightarrow a\textcolor{#61AFEF}{..ab..ab..ab..ab..}b\not\in L$$
>
>Pertanto una contraddizione è inevitabile, implicando che la stringa $a^pb^p$ non può essere iterata e quindi che $L$ non è regolare (non soddisfa il pumping lemma).
>
>Da notare che possiamo semplificare questo ragionamento applicando la condizione 3 del pumping lemma per eliminare i casi 2 e 3.


> [!example]- <font color="orange">Esempio</font>
>Sia $A=\{ww:w\in\{0,1\}^*\}$. Mostriamo che $A$ non è regolare.
>
>Supponiamo per assurdo che $A$ sia regolare. 
>
>> [!quote] RISCRIVERE QUI IL PUMPING LEMMA
>> Se $A$ è un linguaggio regolare, allora esiste un numero $p>0$ (la lunghezza del pumping) tale che se $s$ è una qualsiasi stringa in $A$ di lunghezza almeno $p$ (ovvero che $|s|\geq p$), allora $s$ può essere divisa in tre parti, $s=\textcolor{#e86162}x\textcolor{#61AFEF}y\textcolor{#fdaa39}z$, soddisfacenti le seguenti condizioni:
>> 1. $\forall i\geq0$, $\textcolor{#e86162}x\textcolor{#61AFEF}{y^i}\textcolor{#fdaa39}z\in A$
>> 2. $|\textcolor{#61AFEF}y|>0$
>> 3. $|\textcolor{#e86162}x\textcolor{#61AFEF}y|\leq p$
>
>Sia $s=0^p1\ 0^p1\in A$, con $|s|=2p+2$ e $|s|\geq p$.
>Mostriamo che per ogni $x,y,z\in\{0,1\}^*$ tali che $s=xyz$ con $|xy|\leq p, |y|>0$ si ha che $\exists i\geq0 \text{ t.c. } xy^iz\not\in A$.
>
>Siccome $|xy|\leq p$ e $|y|>0$, allora $y=0^k$, con $1\leq k\leq p$.
>
>Consideriamo $i=2$ e vediamo che $xy^iz=xyyz=0^p0^k1\ 0^p1\not\in A$. Infatti $p+k\neq p$, quindi $0^{p+k}1\ 0^p1$ non si può scrivere come concatenazione di due stringhe uguali.
>
>Abbiamo trovato un assurdo, perciò $A$ non è regolare.


> [!example]- <font color="orange">Esempio</font>
>Sia $A=\{xcx:x\in\{a,b\}^*\}$, dove $c$ è il carattere $c$. Mostriamo che $A$ non è regolare.
>
>Supponiamo per assurdo che $A$ è regolare.
>
>Se $A$ è un linguaggio regolare, allora esiste una $p>0$ tale che una qualsiasi stringa $s$ in $A$ con $|s|\geq p$ può essere divisa in tre parti $xyz$. Dove:
>1. $\forall i\geq0, xy^iz\in A$
>2. $|y|>0$
>3. $|xy|\leq p$
>
>Sia $s=a^pca^p, |s|=2p+1$. Mostriamo che per ogni $s=xyz$ esiste $i\geq 0$ tale che $xy^iz\not\in A$.
>
>Siccome $|xy|\leq p$ e $|y|\geq1$, allora $y=a^k$ con $1\leq k\leq p$.
>
>Consideriamo $i=2$ e vediamo che $xyyz=a^pa^k\ c\ a^p=a^{p+k}\ c\ a^p\not\in A$. Infatti, $p+k\neq p$ e quindi si è trovato l'assurdo, pertanto $A$ non è regolare.


> [!example]- <font color="orange">Esempio Regolare</font>
>Sia $A=\{a^kba^k:k<4\}$.
>
>Questo linguaggio è regolare, in quanto è finito. Quindi esiste un DFA e una espressione regolare che denotano il linguaggio $A$.
>
>Le stringhe appartenenti al linguaggio sono le seguenti: $\{b,aba,aabaa,aaabaaa\}$. (Elencando le stringhe non si deve specificare la RE o il DFA). (NON SCRIVERE MAI CHE È POSSIBILE RAPPRESENTARLO ATTRAVERSO UN NFA, in quanto il DFA è più formale).

# 
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
- [lydia - Video, Pumping Lemma](https://www.youtube.com/watch?v=qtnNyUlO6vU&list=PLhqug0UEsC-IDomfNsn8e3neoy34o8oye&index=6)
- [lydia - Video, Applicazione del Pumping Lemma](https://www.youtube.com/watch?v=Ph7Z9YttM0Q&list=PLhqug0UEsC-IDomfNsn8e3neoy34o8oye&index=7)