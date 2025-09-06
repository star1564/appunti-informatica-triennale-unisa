---
aliases:
  - Media Campionaria
tags:
  - corsi/matematica/probabilità_statistica
inglese: 
paragrafo: Somme di V.A.
cssclasses:
  - 
---
> [!note]+ Valore Atteso di una funzione di due Variabili Aleatorie Discrete
>Siano $X$ e $Y$ due [[025 Variabili Aleatorie Discrete|Variabili Aleatorie]] e $g:\mathbb{R}^2\to \mathbb{R}$ una funzione di due variabili. Se $X$ e $Y$ hanno [[040 Funzioni di Distribuzione Congiunte|Densità Discreta Congiunta]] $p(x,y)$ allora il [[026 Valore Atteso di una VA Discreta|Valore Atteso]] sarà il seguente $$E[g(X,Y)]=\sum_{x}\sum_{y}g(x,y)\cdot p(x,y)$$

## Valore Atteso di una Somma
>Se abbiamo due variabili aleatorie $X,Y$ e se $E[X]$ e $E[Y]$ sono entrambe finite e poniamo $g(x,y)=x+y$, allora abbiamo che
>
>$$\begin{align*} \color{#CC241D}E[X+Y] &= \sum_{x}\sum_{y}(x+y)\cdot p(x,y) \\  &=\sum_{x}x\sum_{y}p(x,y)+\sum_{y}y\sum_{x}p(x,y) \\ &= \sum_{x}x\cdot p_{X}(x)+\sum_{y}y\cdot p_{Y}(y) \\  &=\color{#CC241D}E[X]+E[Y] \end{align*} $$

## Media Campionaria

Consideriamo una sequenza di variabili aleatorie $X_1, \dots, X_n$, che sono:
- *[[041 Variabili Aleatorie Indipendenti|Indipendenti]]*
- *Identicamente distribuite*: tutte seguono la stessa [[024 Funzione di Distribuzione|distribuzione]] di probabilità, indicata con $F$, e hanno lo stesso valore atteso, denotato con $\mu$.

Questa sequenza di variabili rappresenta un *campione casuale* prelevato dalla distribuzione $F$. 

>La **media campionaria**, indicata con $\overline{X}$, è una misura statistica che descrive il valore medio di questo campione. Essa è calcolata sommando tutte le osservazioni del campione e dividendo il risultato per il numero totale di osservazioni $n$.
>$$\color{#CC241D}\overline{X} = \frac{1}{n} \sum_{i=1}^{n} X_i$$

Quindi, $\overline{X}$ è la media aritmetica delle variabili aleatorie $X_1, \dots, X_n$.

Dato che $E[X_i]=\mu$, per la [[026 Valore Atteso di una VA Discreta#Proprietà di Linearità|Proprietà di Linearità]] si ha che il valore atteso è 
$$\color{#61AFEF}E[{\overline{{X}}}]=E\left[{\frac{1}{n}}\sum_{i=1}^{n}X_{i}\right]={\frac{1}{n}}E\left[\sum_{i=1}^{n}X_{i}\right]={\frac{1}{n}}\sum_{i=1}^{n}E[X_{i}]={\frac{1}{n}}n\mu=\mu$$

Pertanto, il valore atteso della media campionaria è $\mu$, ossia la media stessa della distribuzione. La media campionaria è spesso utilizzata per dare una stima del valore atteso $\mu$ della distribuzione, se questo è sconosciuto.

## Disuguaglianza di Boole

Consideriamo una serie di [[011 Evento|eventi]] $A_1, \dots, A_n$. Per ciascuno di questi eventi, definiamo una [[028 Distribuzione di Bernoulli#^cb35ca|variabile indicatrice]] $X_i$, dove:

$$X_i = \begin{cases}  1, & \text{se l'evento } A_i \text{ si verifica} \\ 0, & \text{altrimenti} \end{cases}  \quad \text{per } i=1, \dots, n$$

Questa variabile indicatrice $X_i$ è una [[028 Distribuzione di Bernoulli|variabile di Bernoulli]]. Le proprietà di $X_i$ sono:
- $\mathbb{P}(X_i=1) = \mathbb{P}(A_i)$, che rappresenta la probabilità che l'evento $A_i$ si verifichi
- $\mathbb{P}(X_i=0) = \mathbb{P}(\overline{A_i})$, che rappresenta la probabilità che l'evento $A_i$ *non* si verifichi
- Il valore atteso  di $X_i$ è $E[X_i] = \mathbb{P}(X_i=1) = \mathbb{P}(A_i)$

Ora sommiamo tutte le variabili indicatrici:

$$X = \sum_{i=1}^{n} X_i$$

Questa nuova variabile $X$ rappresenta il *numero totale di eventi $A_i$ che si verificano*.

Introduciamo ora una nuova variabile, $Y$, definita come:

$$Y = \begin{cases} 1, & \text{se almeno uno degli eventi } A_i \text{ si verifica} \\ 0, & \text{altrimenti} \end{cases}$$

In questo caso, $Y$ è anche una variabile di Bernoulli che vale 1 *se almeno uno degli eventi $A_i$ si realizza*, e 0 se nessuno di essi si verifica. È possibile osservare che $Y$ è uguale al *massimo tra le variabili indicatrici* $X_1, \dots, X_n$:

$$Y = \max\{X_1, \dots, X_n\}$$

Ora, calcoliamo il valore atteso di $X$ e di $Y$:

1. Utilizzando la [[026 Valore Atteso di una VA Discreta|proprietà di linearità]], otteniamo
   $$E[X] = E\left[\sum_{i=1}^{n} X_i\right] = \sum_{i=1}^{n} E[X_i] = \sum_{i=1}^{n} \mathbb{P}(A_i)$$

2. Poiché $Y$ è una variabile di Bernoulli, il suo valore atteso è semplicemente la probabilità che valga 1, ossia che si verifichi almeno uno degli eventi $A_i$:
   $$E[Y] = \mathbb{P}(Y=1) = \mathbb{P}(\text{si verifica almeno uno degli eventi } A_i) = \mathbb{P}\left(\bigcup_{i=1}^n A_i\right)$$

Poiché $Y \leq X$ (essendo $Y$ uguale a 1 solo se almeno uno degli $X_i$ è 1, e $X$ conta effettivamente quanti di questi eventi si verificano), possiamo concludere che:
$$E[Y] \leq E[X]$$

>Questo porta alla **Disuguaglianza di Boole**:
>
>$$\color{#CC241D}\mathbb{P}\left(\bigcup_{i=1}^n A_i\right) \leq \sum_{i=1}^{n} \mathbb{P}(A_i)$$
>
>In pratica, la probabilità che almeno uno degli eventi $A_i$ si verifichi è sempre minore o uguale alla somma delle probabilità dei singoli eventi.

## Valore Atteso per le Distribuzioni
### Binomiale
Sia $X$ una variabile aleatoria [[029 Distribuzione Binomiale np|binomiale np]]. Possiamo esprimere $X$ come la somma di $n$ variabili aleatorie di Bernoulli indipendenti $X_1,X_2,\dots,X_n$, dove ciascuna $X_i$​ indica se la $i$-esima prova è stata un successo: $$X=X_1+X_2+\dots+X_n$$
La variabile aleatoria $X_i$​ è definita come: $$X_i = \begin{cases}  1, & \text{se l'}i\text{-esima prova è un successo} \\ 0, & \text{se l'}i\text{-esima prova è un fallimento} \end{cases}$$

Essendo una variabile di Bernoulli, $X_i$​ ha un valore atteso dato da: $$E[X_i]=0\cdot(1-p)+1\cdot p=p$$
Di conseguenza, facendo uso della proprietà di linearità, risulta che
$$E[X]=E\left[\sum_{i=1}^{n}X_{i}\right]=E[X_{1}]+E[X_{2}]+\cdot\cdot\cdot+E[X_{n}]=\underbracket{p+p+\dots+p}_{n \text{ volte}}=n\cdot p$$


#### Somme di Variabili Aleatorie Binomiali indipendenti
Se $X$ e $Y$ sono due variabili aleatorie [[029 Distribuzione Binomiale np|Binomiali]] indipendenti di parametri $(n,p)$ e $(m,p)$ rispettivamente, mostrare che $X+Y$ è binomiale di parametri $(n+m, p)$.

Per $k=0,1,\dots, n+m$
$$
\begin{align*}
\mathbb{P}(X+Y=k) &= \sum_{i=0}^{n}\mathbb{P}\left(X=i,Y=k-i\right) \\
&= \sum_{i=0}^{k}\mathbb{P}\left(X=i\right)\cdot \mathbb{P}\left(Y=k-i\right) \\
&= \sum_{i=0}^{k}{\binom{n}{i}}p^{i}(1-p)^{n-i}{\binom{m}{k-i}}p^{k-i}(1-p)^{m-k+i} \\
&= p^{k}(1-p)^{n+m-k}\sum_{i=0}^{k}{\binom{n}{i}}{\binom{m}{k-i}} \\
&= {\binom{n+m}{k}}p^{k}(1-p)^{n+m-k}
\end{align*}
$$

Avendo utilizzato la [[006 Coefficiente Binomiale#Uguaglianza di Vandermonde|formula di Vandermonde]].

### Distribuzione Normale
Se $X_i, i=1,2,\ldots,n$ sono variabili aleatorie indipendenti con [[038 Distribuzione Normale|Distribuzione Normale]] di parametri $\mu_i$ e $\sigma_i^2$, allora $Y=\sum_{i=1}^{n} X_i$ è una variabile aleatoria normale di parametri $\sum_{i=1}^{n} \mu_i$ e $\sum_{i=1}^{n} \sigma_i$. Quindi, risulta in questo caso che il [[026 Valore Atteso di una VA Discreta|Valore Atteso]] e la [[027 Varianza di una VA Discreta|Varianza]] sono
$$E[Y]=\sum_{i=1}^{n} \mu_i=\sum_{i=1}^{n} E[X_i],\quad \text{Var}(Y)=\sum_{i=1}^{n} \sigma_i=\sum_{i=1}^{n} \text{Var}(X_i)$$

### Poisson
>Se $X$ e $Y$ sono due variabili aleatorie di [[030 Distribuzione di Poisson|Poisson]] indipendenti di parametri $\lambda_1$ e $\lambda_2$ rispettivamente, si calcoli la densità discreta di $X+Y$.

Per $n=0,1,\dots$
$$
\begin{align*}
\mathbb{P}(X+Y=n) &= \sum_{k=0}^{n}\mathbb{P}\left(X=k,Y=n-k\right) \\
&= \sum_{k=0}^{n}\mathbb{P}\left(X=k\right)\cdot \mathbb{P}\left(Y=n-k\right) \\
&= \sum_{k=0}^{n}\mathrm{e}^{-\lambda_{1}}\!\frac{\lambda_{1}^{k}}{k!}\mathrm{e}^{-\lambda_{2}}\!\frac{\lambda_{2}^{n-k}}{(n-k)!} \\
&= \mathrm{e}^{-(\lambda_{1}+\lambda_{2})}\sum_{k=0}^{n}\frac{\lambda_{1}^{k}\lambda_{2}^{n-k}}{k!\left(n-k\right)!} \\
&= \frac{\mathrm{e}^{-(\lambda_{1}+\lambda_{2})}}{n!}\sum_{k=0}^{n}\frac{n!}{k!\left(n-k\right)!}\lambda_{1}^{k}\,\lambda_{2}^{n-k} \\
&= \frac{\mathrm{e}^{-(\lambda_{1}+\lambda_{2})}}{n!}\sum_{k=0}^{n}{n\choose k}\lambda_{1}^{k}\,\lambda_{2}^{n-k} \\
&= \frac{\mathrm{e}^{-(\lambda_{1}+\lambda_2)}}{n!}(\lambda_1+\lambda_2)^n
\end{align*}

$$

Si ricava che $X+Y$ ha distribuzione di Poisson di parametro $\lambda_1+\lambda_2$.




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