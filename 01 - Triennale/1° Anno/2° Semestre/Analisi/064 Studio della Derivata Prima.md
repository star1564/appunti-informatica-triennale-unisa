---
aliases:
  - Punto Angoloso
  - Punto di Flesso a Tangente Verticale
  - Cuspide
tags:
  - corsi/matematica/analisi
paragrafo: Studio di Funzione
cssclasses:
  - 
---
> [!info] Nota (6/7) sullo [[058 Studio di una Funzione|Studio di una Funzione]]
>[[063 Studio degli Asintoti e dei Limiti agli estremi del dominio|<- Passo precedente]] | [[065 Studio della Derivata Seconda|Prossimo passo ->]]

Questo passaggio permette di ottenere informazioni precise riguardo la [[015 Funzione Monotona|monotonia della funzione]], ovvero su quali intervalli del dominio la funzione cresce o scende.

## Calcolare la Derivata Prima

Per iniziare bisogna prima vedere se una funzione è [[046 Derivata di una Funzione#Funzione Derivabile|derivabile]], per poi [[048 Calcolo delle Derivate di una Funzione|calcolarne la derivata prima]] con i metodi standard $$f'(x)$$

## Studiare il Segno della Derivata Prima
Adesso bisogna risolvere la disequazione $$f'(x)\geq 0$$per trovare i segni della derivata.


> [!note] Modo alternativo
> Oppure si risolvono i seguenti calcoli:
> - $f'(x)=0$ (Per vedere se ci sono punti in cui la retta tangente è parallela all'asse delle $x$)
> - $f'(x)>0$ (Per vedere dove è strettamente positiva la derivata)
> - $f'(x)<0$ (Per vedere dove è strettamente negativa la derivata)


Negli intervalli in cui la derivata prima è **positiva**, *$f(x)$ è crescente*, altrimenti se è **negativa** *$f(x)$ è decrescente*.

## Punti di Minimo, Massimo e di Flesso
Vediamo prima nel caso la **derivata prima si annulla** in un punto $x_0$, ovvero poniamo $f'(x_0)=0$, ci sono *quattro* possibilità:
- $x_0$ è un [[049 Massimi e Minimi Relativi e Assoluti#Minimo Relativo (o Locale)|punto di minimo relativo della funzione]] se la derivata è negativa in un [[009 Intorno di un Punto|introno]] sinistro e positiva in un intorno destro del punto.
  
  ![[Pasted image 20220505091554.png]]
- $x_0$ è un [[049 Massimi e Minimi Relativi e Assoluti#Massimo Relativo (o Locale)|punto di massimo relativo della funzione]] se la derivata è positiva in un introno sinistro e negativa in un intorno destro del punto.
  
  ![[Pasted image 20220505091709.png]]
- $x_0$ è un [[057 Derivata Seconda#^39c23f|punto di flesso a tangente orizzontale]] ascendente se la derivata è positiva in un intorno sinistro e destro del punto.
  
  ![[Pasted image 20220505091751.png]]
- $x_0$ è un [[057 Derivata Seconda#^39c23f|punto di flesso a tangente orizzontale]] discendente se la derivata è negativa in un intorno sinistro e destro del punto.
  
  ![[Pasted image 20220505091811.png]]

### Punti in cui la Derivata non è definita
Se la derivata prima non è definita in $x_0$, ovvero $\not\exists f'(x_0)$, ci sono tre possibilità che si possono verificare osservando il limite destro e sinistro del punto alla derivata:
- $x_0$ è un **Punto Angoloso**: 
  $$\lim\limits_{x \to x^-}f'(x)\textcolor{#61AFEF}\neq\lim\limits_{x \to x^+}f'(x)$$
  ![[Pasted image 20220505092918.png]]
- $x_0$ è un **Punto di Flesso a Tangente Verticale**: 
  $$\lim\limits_{x \to x^-}f'(x)=\lim\limits_{x \to x^+}f'(x)=\color{#61AFEF}\pm\infty$$
  ![[Pasted image 20220505092931.png]]
- $x_0$ è una **Cuspide**: $\lim\limits_{x \to x^-}f'(x)$ e $\lim\limits_{x \to x^+}f'(x)$ *fanno uno $+\infty$ e l'altro $-\infty$ o viceversa*.
  ![[Pasted image 20220505092949.png]]

---
# %%Caso Studio%%

> [!example]+ <font color="orange">Esempio Caso Studio</font>
> $$y=\frac{\ln(x+1)}{-2-\ln(x+1)}$$$$Dom(f)=(-1, e^{-2}-1)\ \cup\ (e^{-2}-1, +\infty)$$
> La funzione è derivabile in ogni punto del dominio, in quanto composizione e rapporto di funzioni che sono derivabili in ogni punto del dominio.
> $$Dom(f')=Dom(f)$$
> Calcoliamo la derivata prima della funzione (dove si usa [[048 Calcolo delle Derivate di una Funzione#Rapporto di due Funzioni|la regola della derivata di un rapporto di due funzioni]]):
> 
> $$
> \begin{align*}
> f'(x) &=\frac{d}{dx}\left[\frac{\ln(x+1)}{-2-\ln(x+1)}\right] \\
> &=\frac{\frac{d}{dx}[\ln(x+1)]\cdot (-2-\ln(x+1))- \ln(x+1)\cdot \frac{d}{dx}[-2-\ln(x+1)]}{(-2-\ln(x+1))^2} \\
> &=\frac{\frac{1}{x+1}\cdot (-2-\ln(x+1)) - \ln(x+1)\cdot (\frac{d}{dx}[-2]-\frac{d}{dx}[\ln(x+1)])}{(-2-\ln(x+1))^2} \\
> &=\frac{\frac{1}{x+1}\cdot (-2-\ln(x+1)) - \ln(x+1)\cdot (-\frac{1}{x+1})}{(-2-\ln(x+1))^2} \\
> &=\frac{\frac{-2-\ln(x+1)}{x+1} + \frac{\ln(x+1)}{x+1}}{(-2-\ln(x+1))^2} \\
> &=\frac{\frac{-2-\ln(x+1)+\ln(x+1)}{x+1}}{(-2-\ln(x+1))^2} \\
> &=\frac{\frac{-2}{x+1}}{(-2-\ln(x+1))^2} \\
> &=\frac{-2}{x+1}\cdot\frac{1}{(-2-\ln(x+1))^2} = \frac{-2}{(x+1)\cdot(-2-\ln(x+1))^2} \\
> \end{align*}
> $$
> 
> 
> 
> 
> Poniamo $f'(x)=0$ per vedere se si ha un punto di massimo o di minimo:
> $$\frac{-2}{(x+1)\cdot(-2-\ln(x+1))^2}=0\iff-2=0\iff\text{Impossibile}$$
> Non ammette soluzioni, quindi la funzione <font color="orange">non ha un punto di massimo o di minimo</font>.
> 
> Studiamo il segno della derivata prima ponendo $f'(x)\geq 0$:
> $$\begin{cases} -2\geq0 \\ (x+1)\cdot(-2-\ln(x+1))^2>0 \end{cases}\iff\begin{cases} \text{Impossibile (\textcolor{#a6d189}{linea tratteggiata})} \\ \text{Sempre} > 0 \text{ (\textcolor{#99d1db}{linea piena})} \end{cases}$$
> 
> La seconda disequazione si può dividere in due, la prima riporta che $x>1$, l'altra è un quadrato, quindi è sempre positiva.
> 
> Il grafico dei segni per la disequazione è il seguente:
> 
> ![[Pasted image 20241005150407.png|900]]
> 
> Quindi la funzione <font color="orange">decresce su tutto il proprio dominio e non sono presenti punti di massimo né di minimo</font>.

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
- [YouMath](https://www.youmath.it/lezioni/analisi-matematica/studio-di-funzioni-grafico/182-step-6-studio-della-derivata-prima-massimi-e-minimi-assoluti-e-relativi-monotonia.html)