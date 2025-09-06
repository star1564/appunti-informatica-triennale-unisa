---
tags:
  - corsi/matematica/probabilità_statistica
paragrafo: Probabilità
cssclasses:
  - 
aliases:
---
>Si definisce **Probabilità Condizionata** la probabilità che si verifichi un [[011 Evento|Evento]] $A$ *sapendo che si è già verificato un altro evento* $B$.  
>
>Se $\mathbb{P}(B)>0$, la probabilità condizionata di $A$ dato $B$ è 
>$$\color{#CC241D}\mathbb{P}(A|B)=\frac{\mathbb{P}(A\cap B)}{\mathbb{P}(B)}$$

> [!info]+ Probabilità condizionata con un diagramma di Venn
> Supponiamo di avere 9 biglie e due eventi $A$ e $B$ con un overlap. Diciamo che l'evento $B$ è accaduto, quindi tutte le biglie al di fuori di $B$ sono "eliminate".
> Vogliamo conoscere la probabilità di $A$ dopo questo avvenimento, ovvero $\mathbb{P}(A|B)$.
> 
> ![[Pasted image 20240228145024.png|400]]
>
> - Per fare questo si aggiorna la probabilità di $A$, prendendo l'intersezione $A\cap B$. In questo modo si restringe l'universo delle possibilità a quelle che ci interessano.
> - Si divide questo valore per $\mathbb{P}(B)$ per bilanciare nuovamente la probabilità totale del nuovo universo ad 1.

> [!quote]+ Dimostrazione
> Dalla definizione di [[016 Definizione di Probabilità|Probabilità classica]] abbiamo che $\mathbb{P}(A)=\frac{|A|}{|S|}$. Volendo esprimere in tale ambito la probabilità condizionata di $A$ dato $B$, siamo condotti ad usare il rapporto di casi favorevoli al verificarsi di $A$ (sapendo che si è verificato $B$) su casi possibili (gli elementi di $B$), cosicché: 
> $$\begin{align}\mathbb{P}(A|B)&=\frac{|A\cap B|}{|B|}\\ &=\frac{|A\cap B|/|S|}{|B|/|S|}\\ &=\frac{\mathbb{P}(A\cap B)}{\mathbb{P}(B)}\end{align}$$
> Questa dimostrazione è valida quando gli esiti sono equiprobabili.



> [!example]- <font color="orange">Esempio</font>
> Nell'esperimento del lancio di un dado non truccato calcolare le probabilità condizionate di $A=\{1,2\}$ dati gli eventi $B_1=\{4,5,6\},B_2=\{1,5,6\},B_3=\{1,2,6\}$.
>
> Abbiamo che: $$\mathbb{P}(A)=\frac{2}{6}=\frac{1}{3},\quad\quad \mathbb{P}(B_i)=\frac{3}{6}=\frac{1}{2},\quad i=1,2,3$$
> Allora: 
> - Con $A\cap B_1 = \{1,2\}\cap\{4,5,6\}=\emptyset$: $$\mathbb{P}(A|B_1)=\frac{\mathbb{P}(A\cap B_1)}{\mathbb{P}(B_1)}=\frac{\mathbb{P}(\emptyset)}{1/2}=0$$
> 
> 
> - Con $A\cap B_2 = \{1,2\}\cap\{1,5,6\}=\{1\}$ e la probabilità $\mathbb{P}(\{1\})=\frac{1}{6}$: $$\mathbb{P}(A|B_2)=\frac{\mathbb{P}(A\cap B_2)}{\mathbb{P}(B_2)}=\frac{\mathbb{P}(\{1\})}{1/2}=\frac{1/6}{1/2}=\frac{1}{3}$$
> 
> - Con $A\cap B_3 = \{1,2\}\cap\{1,2,6\}=\{1,2\}$ e la probabilità $\mathbb{P}(\{1,2\})=\frac{2}{6}=\frac{1}{3}$: $$\mathbb{P}(A|B_3)=\frac{\mathbb{P}(A\cap B_3)}{\mathbb{P}(B_3)}=\frac{\mathbb{P}(\{1, 2\})}{1/2}=\frac{1/3}{1/2}=\frac{2}{3}$$

## Proprietà e Teoremi
### Assiomi della Probabilità
La probabilità condizionale *è di per sé una misura di probabilità*, quindi soddisfa gli [[017 Assiomi della Probabilità|assiomi della probabilità]]:
- $0\leq \mathbb{P}(A|B) \leq 1$
- $\mathbb{P}(S|A) = 1$
- $P\big(\bigcup_{i=1}^\infty F_i|A\big) = \sum_{i=1}^{\infty} \mathbb{P}(F_i|A)$

Si possono applicare tutte le loro [[017 Assiomi della Probabilità#Conseguenze degli assiomi della Probabilità|conseguenze]].

### Probabilità Condizionata con Eventi Disgiunti
>Quando due eventi sono [[013 Eventi Compatibili, Incompatibili e Complementari#Eventi Incompatibili (o Disgiunti)|Disgiunti]], ovvero che $E\cap F = \emptyset$, allora:
> $$\begin{align}
 \mathbb{P}(E|F)&=\frac{\mathbb{P}(E\cap F)}{\mathbb{P}(F)} \\
 &= \frac{\mathbb{P}(\emptyset)}{\mathbb{P}(F)} \\
 &= 0
 \end{align}$$

Questo ha senso. In particolare, poiché $E$ e $F$ sono disgiunti, non possono verificarsi entrambi nello stesso momento. Pertanto, dato che $F$ si è verificato, la probabilità di $E$ deve essere pari a zero.

### Probabilità Condizionata con Sottoinsiemi
> Quando $F$ è un [[002 Sottoinsiemi|sottoinsieme]] di $E$, ovvero che $F\subseteq E$, allora quando $F$ accade, accadrà anche $E$. 
> Pertanto, dato che $F$ è accaduto, si ci aspetta che la probabilità di $E$ sia $1$, dato che $E\cap F=F$:
> $$\begin{align}
 \mathbb{P}(E|F)&=\frac{\mathbb{P}(E\cap F)}{\mathbb{P}(F)} \\
 &= \frac{\mathbb{P}(F)}{\mathbb{P}(F)} \\
 &= 1
 \end{align}$$

#### Sottoinsieme Opposto
> Se invece si ha che $E\subseteq F$, allora si ha che $E\cap F=E$, pertanto:
> $$\begin{align}
 \mathbb{P}(E|F)&=\frac{\mathbb{P}(E\cap F)}{\mathbb{P}(F)} \\
 &= \frac{\mathbb{P}(E)}{\mathbb{P}(F)} \\
 \end{align}$$

### Formula delle alternative
>Sia $F$ un evento, tale che $0<\mathbb{P}(F)<1$. Se $E$ è un evento qualsiasi risulta che:
>$$\color{#CC241D}\mathbb{P}(E)=\mathbb{P}(E|F)\cdot \mathbb{P}(F)+\mathbb{P}(E|\overline{F})\cdot \mathbb{P}(\overline{F})$$

> [!quote]+ Dimostrazione
> ![[Pasted image 20230522104525.png]]
> Guardando l'immagine, l'evento $E$ si può esprimere come $\mathbb{P}(E)=(E\cap F)\cup (E\cap\overline{F})$.

#### Formula delle alternative con $n$ alternative
Se gli eventi $F1, F2, \dots , Fn$ sono [[013 Eventi Compatibili, Incompatibili e Complementari#Eventi Due a Due Incompatibili (o Disgiunti)|Eventi Due a Due Incompatibili]], [[011 Evento#^3a631f|necessari]], e ciascuno con probabilità positiva, e se $E$  è un evento qualsiasi, allora risulta: 
$$\mathbb{P}(E)=\displaystyle\sum_{i=1}^{n} \mathbb{P}(E|F_{i})\cdot \mathbb{P}(F_{i})$$
Permette quindi di determinare la probabilità di un evento condizionandolo prima alla realizzazione di uno, e uno solo, degli $n$ eventi $F1, F2, \dots , Fn$.

# 
___
[[000 Indice PS|↖ Ritorna all'indice ↖]]

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
- [YouMath](https://www.youmath.it/lezioni/probabilita/probabilita-discreta/1206-probabilita-condizionata.html)
- [Video (inglese), Statistics 110 - Probabilità Condizionale](https://www.youtube.com/watch?v=P7NE4WF8j-Q&list=PL2SOU6wwxB0uwwH80KTQ6ht66KWxbzTIo&index=4)
- https://www.probabilitycourse.com/chapter1/1_4_0_conditional_probability.php