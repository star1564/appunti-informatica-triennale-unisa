---
aliases:
  - Integrale secondo Riemann
  - Integrale Definito
tags:
  - corsi/matematica/analisi
paragrafo: Integrali
cssclasses:
  - 
---
>Un **Integrale** sull'[[004 Intervalli#Intervallo Chiuso|intervallo]] $[a,b]$ di una funzione $f(x)$, è l'*area (con segno)* della regione di piano *compresa tra il grafico di $f(x)$, l'asse $x$ e le rette verticali $x=a, x=b$.*
>
>L'integrale si denota con: $$\color{#CC241D}\int_a^b f(x)\,dx$$

Dove:
- $\int_\textcolor{#61AFEF}a^\textcolor{#61AFEF}b$ è l'<font color="#61AFEF">Intervallo di Integrazione</font>, ovvero $[a,b]$
- $\color{#b35db0}f(x)$ è la <font color="#b35db0">funzione integranda</font>, ovvero la funzione che si sta prendendo in considerazione nel calcolo dell'integrale
- La variabile $\color{green}dx$, è il <font color="green">differenziale</font>, ovvero un pezzo infinitamente piccolo dell'asse $x$. Serve a dire che si sta sommando tanti piccoli segmenti lungo l'asse $x$ per calcolare l'area complessiva ([[#Osservazione|osservazione alla fine]]) ^7b8a91

![[Immagine 2022-05-07 0ddadpng 1.png|500]]




Se la regione di piano che si andrà a prendere in considerazione si trova al di sotto dell'asse delle $x$, l'integrale equivale all'area *con segno negativo*.

## Definizione di Integrale secondo Riemann
### Funzioni Costanti
Considerare una [[F01 Funzione Lineare e Costante#^759638|funzione costante]] $$f(x)=k,\ \forall x\in[a,b], k\in \mathbb{R}$$
Allora l'integrale di $f$ sull'intervallo $[a,b]$ è il seguente: $$\int_a^b f(x)\,dx=\textcolor{#61AFEF}{(b-a)}\cdot \textcolor{#B35DB0}k$$
Ovvero, $\textcolor{#61AFEF}{\text{base}}\cdot \textcolor{#B35DB0}{\text{altezza}}$, dove:
- La base del rettangolo è la distanza tra $a$ e $b$, ovvero $\textcolor{#61AFEF}{b-a}$.
- L'altezza (con segno) del rettangolo è la costante $\textcolor{#B35DB0}k$.

Questo coincide all'<font color="orange">area</font> (con segno) del rettangolo nel seguente grafico.

![[Pasted image 20220507103014.png]]



### Funzioni a Scala
Sia $f(x)$ una funzione a scala (che assume il valore $k_i$ nell'i-esimo intervallo avente $x_{i-1}$ ed $x_i$ come estremi), allora il suo integrale sull'intervallo $[a,b]$ è: $$\int_a^b f(x)\,dx=\sum^{n}_{i=1}(x_i-x_{i-1})\cdot k_i$$
Ovvero, l'integrale è la [[Sommatoria]] di tutte le aree (con segno) dei rettangoli, come quelli in figura.

![[Pasted image 20220507103853.png]]

Infatti $x_i-x_{i-1}$ è la base dell'i-esimo rettangolo, mentre $k_i$ è la sua altezza.

L'area di ogni segmento della funzione a scala viene chiamato **partizione**.

### Funzione Qualsiasi e Integrale Definito
Prendere in considerazione una qualsiasi funzione $f(x)$, allora si può *approssimare l'area che equivale al suo integrale* in due modi: 
- Creando una funzione a scala $\textcolor{green}{h(x)}$, tale che $\textcolor{green}{h(x)}\textcolor{#61AFEF}\geq f(x),\forall x\in[a,b]$.
  
  ![[Pasted image 20220507105206.png|400]]
- Creando una funzione a scala $g(x)$, tale che $\textcolor{#756cff}{g(x)}\textcolor{#61AFEF}\leq f(x),\forall x\in[a,b]$.
  
  ![[Pasted image 20220507105401 1.png|400]]

In questi due casi si ha che l'area è compresa tra l'[[007 Estremo Inferiore e Superiore di un insieme#ESTREMO SUPERIORE|estremo superiore]] dell'insieme delle possibili aree trovate con $\textcolor{green}{h(x)}$, e tra l'[[007 Estremo Inferiore e Superiore di un insieme#ESTREMO INFERIORE|estremo inferiore]] dell'insieme delle possibili aree trovate con $\textcolor{#756cff}{g(x)}$: 
$$\sup\left\{\int_a^b \textcolor{#756cff}{g(x)}\,dx\right\}\leq\textcolor{orange}{A}\leq \inf\left\{\int_a^b \textcolor{green}{h(x)}\,dx\right\}$$
Se questi due estremi *coincidono*, si dice che $f$ è **Integrabile secondo Riemann sull'intervallo $\color{#CC241D}[a,b]$** ed il loro valore comune è $$\int_a^b f(x)\,dx$$Questo valore viene detto **Integrale Definito** della funzione $f$ in $[a,b]$.

---
# %%Criterio di Integrabilità%%

> [!quote] <font color="orange">Criterio di Integrabilità [273 Aracne]</font>
>Sia $f:[a,b]\to\mathbb{R}$ una funzione [[006 Insieme Illimitato e Limitato#Insiemi Limitati|limitata]]. 
>Questa è **Integrabile secondo Riemann** se e solo se, per ogni $\epsilon>0$, esiste una partizione $P$ di $[a,b]$ tale il [[MB - 16 Valore Assoluto|valore assoluto]] della differenza tra le somme superiori $S$ e le somme inferiori $s$ è minore di $\epsilon$:
>$$S(P)-s(P)<\epsilon$$
>
>Inoltre, è **integrabile** se $f$ è una funzione [[035 Funzione Continua|continua]].

---
# %%Osservazione%%
> [!note]+ Osservazione sul $dx$ dell'integrale
> Si parla delle possibili aree trovate per le due funzioni perché la base dei rettangoli può variare. Infatti *più la base è piccola, più l'approssimazione dell'area è corretta*. Lo si può notare in questa animazione.
> 
> ![[674c60b998331770b0e4672b2f5a1077.gif|500]]
>
>Praticamente, la base di tutti i rettangoli è la dimensione fissa $dx$, mentre la loro altezza variabile è definita da $f(x)$. Quindi moltiplicando $f(x)\cdot dx$ si ottiene l'area di un rettangolo. Ora si devono solo sommare le aree e si otterrà un'approssimazione dell'integrale.
>
>![[Pasted image 20220507114105.png|600]]

___
[[000 Indice Analisi|↖ Ritorna all'indice ↖]]

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
- Pagina 271 Aracne
- [Video Elia Bombardelli](https://www.youtube.com/watch?v=MOE7x_B_WeA&t=65s)
- [YouMath (Più Complicato)](https://www.youmath.it/lezioni/analisi-matematica/integrali/620-definizione-di-integrale-di-riemann.html)
- [Video 3Blue1Brown (Inglese)](https://www.youtube.com/watch?v=WUvTyaaNkzM&list=PLZHQObOWTQDMsr9K-rj53DwVRMYO3t5Yr&index=1&t=738s)
