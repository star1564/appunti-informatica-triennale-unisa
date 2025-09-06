---
aliases: 
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Probabilità
cssclasses:
  - 
---
>Con **assiomi della Probabilità** ci si riferisce ai tre [[001 Definizioni Varie#Assioma (Postulato)|assiomi]] fondamentali su cui poggia la teoria del Calcolo delle Probabilità. Essi fissano delle regole base che ogni metodo di calcolo delle probabilità deve rispettare.

Consideriamo un [[011 Evento|Evento]] $E$ ed uno [[010 Spazio Campionario|Spazio Campionario]] $S$.

> [!quote] <font color="orange">Assioma 1 - Valore della probabilità</font>
> La probabilità di $E$ è sempre compresa tra $0$ e $1$, estremi inclusi.
> $$\textcolor{#CC241D}{0\leq \mathbb{P}(E)\leq 1},\quad \forall E\subseteq S$$

> [!quote] <font color="orange">Assioma 2 - Probabilità dello spazio campionario</font>
> La probabilità dell'intero spazio campionario, ossia dell'[[011 Evento#^660fb4|Evento Certo]], è uguale a $1$.
> $$\color{#CC241D}\mathbb{P}(S) = 1$$

> [!quote] <font color="orange">Assioma 3 - Unione di due eventi incompatibili</font>
>Per ogni *coppia di [[013 Eventi Compatibili, Incompatibili e Complementari#Eventi Incompatibili (o Disgiunti)|Eventi Incompatibili (o Disgiunti)]]* $E_1,E_2\subseteq S$, la probabilità della loro unione è uguale alla somma delle loro probabilità.
>$$\textcolor{#CC241D}{\mathbb{P}(E_{1}\cup E_{2})=\mathbb{P}(E_{1})+ \mathbb{P}(E_{2})},\quad E_1\cap E_2=\emptyset$$
>
>Se i due eventi sono [[013 Eventi Compatibili, Incompatibili e Complementari#Eventi Compatibili|compatibili]], si usa la [[#Probabilità totale|probabilità totale]].


> [!example]- <font color="orange">Esempio</font>
>>In un'elezione, ci sono quattro candidati. Chiamiamoli $A$, $B$, $C$ e $D$. $A$ ha il $20\%$ di probabilità di vincere le elezioni, mentre $B$ ha il $40\%$ di probabilità di vincere. Qual è la probabilità che $A$ *oppure* $B$ vincano le elezioni?
> 
> Si noti che gli eventi $\{A \text{ vince}\}$, $\{B \text{ vince}\}$, $\{C \text{ vince}\}$ e $\{D \text{ vince}\}$ sono [[013 Eventi Compatibili, Incompatibili e Complementari#Eventi Incompatibili (o Disgiunti)|disgiunti]] poiché più di uno di essi non può verificarsi contemporaneamente. Ad esempio, se $A$ vince, $B$ non può vincere. 
> 
> Dal terzo assioma della probabilità, la probabilità dell'unione di due eventi disgiunti è la somma delle probabilità individuali. Pertanto:
> 
> $$\begin{align}
> \mathbb{P}( A\text{ vince oppure }B\text{ vince} ) &= \mathbb{P}(\{ A\text{ vince}\} \cup \{B \text{ vince} \} ) \\
> &= \mathbb{P}(\{ A\text{ vince}\}) + \mathbb{P}(\{B \text{ vince} \} ) \\
> &= 0.2 + 0.4 \\
> &= 0.6
> \end{align}$$


## Conseguenze degli assiomi della Probabilità
Dai tre assiomi della Probabilità discendono alcune importanti proprietà.

### Probabilità del complementare di un evento
>La probabilità del [[013 Complemento di un Insieme|complementare]] di un evento vale $1$ meno la probabilità dell'evento stesso.
>$$\color{#CC241D}\mathbb{P}(\overline{E})=1-\mathbb{P}(E)$$

> [!success]+ KEYWORD
> Questa proprietà è usata molte volte per risolvere problemi. Deve essere usata quando nella traccia sono presenti le seguenti keywords:
> - "almeno"
> - "altrimenti"
> - "negli altri casi"


> [!quote]- Dimostrazione
>Supponiamo che nello spazio campionario $S$ ci sia un evento $E$.
>Dato che è *sempre vero* che $E\cap \overline{E}=\emptyset$, ovvero che un evento è sempre disgiunto dal suo complemento, allora si ha
> $$\begin{align}
> 1&= \mathbb{P}(S) \\
> &= \mathbb{P}(E\cup \overline{E}) \\
> &= \mathbb{P}(E) + \mathbb{P}(\overline{E})
> \end{align}$$
> 
> Riorganizzando l'equazione si ha la tesi, ovvero che $$\mathbb{P}(\overline{E})=1-\mathbb{P}(E)$$
>
>![[Pasted image 20250311111813.png|300]]




#### Probabilità che accada un evento "almeno una volta in $n$ volte"
>$$\mathbb{P}(\text{almeno una volta accade } E)=1-\mathbb{P}(\overline{E})^n$$

> [!example]- <font color="orange">Esempio: Newton-Pepys</font>
> >Dato un dado non truccato, qual è l'evento con maggiore probabilità di avvenimento tra:
> >1. Almeno un 6 esce tirando 6 dadi
> >2. Almeno due 6 escono tirando 12 dadi
> >3. Almeno tre 6 escono tirando 18 dadi
>
> - $\mathbb{P}(A)=1-\textcolor{#FF6611}{\left( \frac{5}{6} \right)^6}\simeq 0.665$
> Andiamo a calcolare la probabilità inversa di A, ovvero che <font color="#FF6611">non appare alcun 6 nel lancio dei 6 dadi</font>. Quindi $x > 0$ dove $x$ è il numero di risultati diversi da 6 usciti. Questa probabilità è una disposizione con ripetizioni, dove si individua la probabilità che non esca 6 per ogni dado ($x=0$).
> - $\mathbb{P}(B)=1-\textcolor{#FF6611}{\left( \frac{5}{6} \right)^{12}}-\textcolor{#00C575}{12\cdot\left( \frac{1}{6} \right)\cdot\left( \frac{5}{6} \right)^{11}}\simeq 0.619$
> Andiamo a calcolare la probabilità inversa di B, ovvero che <font color="#FF6611">non appare alcun 6 nel lancio dei 12 dadi</font> e che <font color="#00C575">ne esca esattamente uno</font>. Quindi $x > 1$ dove $x$ è il numero di risultati diversi da 6 usciti. La <font color="#FF6611">prima</font> è una disposizione con ripetizioni, dove si individua la probabilità che non esca 6 per ogni dado ($x=0$). La <font color="#00C575">seconda</font> è una disposizione con ripetizioni che si moltiplica alla probabilità che non esca 6 su 11 posti che si moltiplica per il numero di lanci ottenuto da ${12\choose 1}$, si individua la probabilità che per ogni dado esca solamente un 6 ($x=1$). 
> - $\mathbb{P}(C)=1-\textcolor{#FF6611}{\left( \frac{5}{6} \right)^{18}}-\textcolor{#00C575}{18\cdot\left( \frac{1}{6} \right)\cdot\left( \frac{5}{6} \right)^{17}}-\textcolor{#61AFEF}{{18\choose 2}\cdot\left( \frac{1}{6} \right)^2\cdot\left( \frac{5}{6} \right)^{16}}=1-\sum_{k=0}^{2} {18\choose k}\cdot\left( \frac{1}{6} \right)^k\cdot\left( \frac{5}{6} \right)^{18-k}\simeq 0.597$ 
> Andiamo a calcolare la probabilità inversa di C, ovvero che <font color="#FF6611">non appare alcun 6 nel lancio dei 18 dadi</font>, che <font color="#00C575">ne esca esattamente uno nel lancio dei 18 dadi</font> e che <font color="#61AFEF">ne escano esattamente due nel lancio dei 18 dadi</font>. Quindi $x > 2$ dove $x$ è il numero di risultati diversi da 6 usciti. La <font color="#FF6611">prima</font> è una disposizione con ripetizioni, dove si individua la probabilità che non esca 6 per ogni dado ($x=0$). La <font color="#00C575">seconda</font> è una disposizione con ripetizioni che si moltiplica alla probabilità che non esca 6 su 17 posti che si moltiplica per il numero di lanci ottenuto da ${18\choose 1}$, quindi si individua la probabilità che per ogni dado esca solamente un 6 ($x=1$). La <font color="#61AFEF">terza</font> è una disposizione con ripetizioni che si moltiplica alla probabilità che non esca 6 su 16 posti che si moltiplica per il numero di lanci ottenuto da ${18\choose 2}$, si individua la probabilità che per ogni dado escano esattamente due 6 ($x=2$).  


### Probabilità dell'evento impossibile
>La probabilità dell'[[011 Evento#^da36b3|Evento Impossibile]] è $0$.
>$$\mathbb{P}(\emptyset)=0$$

> [!quote]- Dimostrazione
> Dato che $\emptyset = \overline{S}$, si può usare il complemento:
> $$\begin{align}
> \mathbb{P}(\emptyset) &= 1 - \mathbb{P}(\overline{\emptyset}) \\
> &= 1-\mathbb{P}(S) \\
> &= 1-1 \\
> &=0
> \end{align}$$
### Proprietà di Monotonicità
>Se un evento $E$ è incluso in un evento $F$, allora la probabilità di $E$ è minore o uguale alla probabilità di $F$.
>Inoltre, se avviene $E$, avverrà anche $F$.
>$$E\subseteq F\implies \mathbb{P}(E)\leq \mathbb{P}(F)$$

> [!quote]- Dimostrazione
> $$\begin{align}
> \mathbb{P}(F)&=\mathbb{P}(E\cap F) + \mathbb{P}(F-E) \\
> &=\mathbb{P}(E)+\mathbb{P}(F-E) \\
> &\geq \mathbb{P}(E)
> \end{align}$$
> 
> ![[Pasted image 20250319111827.png]]



### Probabilità dell'unione di più eventi incompatibili
>La probabilità dell'unione di $n\geq2$ eventi *[[013 Eventi Compatibili, Incompatibili e Complementari#Eventi Incompatibili (o Disgiunti)|incompatibili]]* è uguale alla somma delle loro probabilità.
> $$\mathbb{P}\Bigg(\displaystyle\bigcup^n_{i=1}E_i\Bigg)=\displaystyle\sum_{i=1}^{n} \mathbb{P}(E_{i})$$
 
Ovvero che:
$$\mathbb{P}(E_{1}\cup E_{2}\cup\dots)=\mathbb{P}(E_{1})+ \mathbb{P}(E_{2})+\dots$$

## Probabilità totale
>Dati due [[013 Eventi Compatibili, Incompatibili e Complementari#Eventi Compatibili|Eventi Compatibili]] $E_1,E_2\subseteq S$, la probabilità della loro unione è data dalla seguente espressione
>$$\color{#CC241D}\mathbb{P}(E_1\cup E_2)=\mathbb{P}(E_1)+\mathbb{P}(E_2)-\mathbb{P}(E_1\cap E_2)$$

Questo accade solamente se i due eventi *hanno un overlap tra di loro*, ovvero quando sono compatibili. 

> [!quote]+ Dimostrazione
> 
>Bisogna rimuovere la probabilità dell'intersezione $E_1\cap E_2$ perché è già stata contata attraverso la somma $\mathbb{P}(E_1)+\mathbb{P}(E_2)$. 
>
>![[Pasted image 20230930171514.png|400]]

> [!example]- <font color="orange">Esempio</font>
>>Uno studente deve sostenere due test. 
>>- Con probabilità $0,5$ supererà il primo test
>>- Con probabilità $0,4$ supererà il secondo test
>>- Con probabilità $0,3$ li supererà entrambi
>>
>>Quanto vale la probabilità che non supererà nessuno dei due test?
>
>Sia $B_i$ l'evento che lo studente superi il test $i$-esimo, con $i = 1, 2$. La probabilità che *superi almeno un test* sarà quindi pari a:
>$$\mathbb{P}(B_1 \cup B_2)= \mathbb{P}(B_1)+\mathbb{P}(B_2)-\mathbb{P}(B_1\cap B_2)=0,5+0,4-0,3=0,6$$
>
>La probabilità che lo studente non supererà nessuno dei due test è dunque:
>$$\mathbb{P}(\overline{B_1} \cap \overline{B_2})=\mathbb{P}(\overline{B_1 \cup B_2})= 1-\mathbb{P}(B_1\cup B_2)=1-0,6=0,4$$
>
>Dove si è usata la [[Leggi De Morgan|Legge di De Morgan]].

### Principio di Inclusione-Esclusione
>Il **Principio di Inclusione-Esclusione** viene usato quando si vuole calcolare la *probabilità dell'unione* di un certo numero di eventi quando *NON sono a due a due incompatibili*.

$$\mathbb{P}\left(\bigcup_{i=1}^n E_i\right) = \sum_{i=1}^n \mathbb{P}(E_i) - \sum_{i<j} \mathbb{P}(E_i \cap E_j) + \sum_{i<j<k} \mathbb{P}(E_i \cap E_j \cap E_k) - \ldots + (-1)^{n+1} \cdot \mathbb{P}(E_1 \cap E_2 \cap \ldots \cap E_n)
$$

> [!example]- <font color="orange">Esempio con n = 3</font>
> $$\mathbb{P}(A∪B∪C)=\mathbb{P}(A)+\mathbb{P}(B)+\mathbb{P}(C)−(\mathbb{P}(A∩B)+\mathbb{P}(A∩C)+\mathbb{P}(B∩C))+\mathbb{P}(A∩B∩C)$$
> 
> ![[Pasted image 20230930180006.png|350]]


> [!example]- <font color="orange">Esempio</font>
> $S=\{1,2,3,4,5,6\}$
> $A=\{1,3,5\}$
> $B=\{2,4\}$
> $C=\{4,5,6\}$
>
>- $A\cap B=\emptyset \quad\to\quad\mathbb{P}(A\cup B)=\mathbb{P}(A)+\mathbb{P}(B)=\frac{3}{6}+\frac{2}{6}=\frac{5}{6}$
>- $A\cap C\neq\{5\} \to\quad\mathbb{P}(A\cup C)=\mathbb{P}(A)+\mathbb{P}(C)-\mathbb{P}(A\cap C)=\frac{3}{6}+\frac{3}{6}-\frac{1}{6}=\frac{5}{6}$


# %%END%%
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
- [YouMath](https://www.youmath.it/lezioni/probabilita/probabilita-discreta/5188-assiomi-di-probabilita.html)
- [Probability Course](https://www.probabilitycourse.com/chapter1/1_3_2_probability.php)