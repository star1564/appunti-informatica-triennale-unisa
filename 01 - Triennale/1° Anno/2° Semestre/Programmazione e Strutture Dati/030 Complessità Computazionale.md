---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Complessità Computazionale
cssclasses:
  - 
---
> [!warning] Argomento trattato nel corso di [[002 Complessità Computazionale|Progettazione Algoritmi]] e [[052 Classi complessità TIME|Elementi di Teoria della Computazione]]



La **Complessità Computazionale** è quella branca dell'analisi degli algoritmi che studia le fondamentali limitazioni incontrate nella loro progettazione.

Lo scopo ultimo è quello di determinare il tempo di calcolo nel caso peggiore di un algoritmo per un dato problema.
Questo tempo di calcolo rappresenta la *Complessità* del problema.

Nell'*Analisi della Complessità* si stima il costo degli algoritmi in termini di risorse di calcolo, come il tempo del completamento o lo spazio di memoria usato.

## MODELLO ASTRATTO
Per stimare il costo temporale di un algoritmo abbiamo bisogno di un **Modello Astratto** che:
- Non tiene conto della macchina utilizzata;
- Stima in funzione della dimensione dell'input;
- Ha un comportamento [[043 Funzioni Asintotiche|asintotico]];
- Stima il caso peggiore della configurazione dei dati input.

In questo modello valgono le seguenti regole:
- Le istruzioni e condizioni atomiche hanno un *costo unario*;
- Le dichiarazioni di variabili non hanno un costo;
```C
int var; //Nessun Costo
```
- Gli assegnamenti di un valore ad una variabile costano 1;
```C
//(Costo = 0)
int var = 5; //Costo++ (Costo = 1)
var = 10; //Costo++ (Costo = 2)
```
- Le strutture di controllo hanno un costo pari alla somma dei costi dell'esecuzione delle istruzioni interne, più la somma dei costi delle condizioni;
```C
//(Costo = 0)
if (var == 10){ //Costo++ (Costo = 1)
	var = 15; //Costo++ (Costo = 2)
	var = 20; //Costo++ (Costo = 3)
}
//Totale con if vero: Costo = 3
//Totale con if falso: Costo = 1
```
```C
//(Costo = 0)
int test = 0; //Costo++ (Costo = 1)
int var = 0; //Costo++ (Costo = 2)
while (var < 3){ //Costo += 3+1 (Costo = 6)
	test++; //Costo += 3 (Costo = 9)
	var++; //Costo += 3 (Costo = 12)
}
//Totale: Costo = 12
```
- Le chiamate a funzione hanno un costo pari al costo di tutte le sue istruzioni e condizioni (inoltre il passaggio dei parametri ha costo nullo);
```C
int fun(int n){
	n++; //Costo_fun++ (Costo_fun = 1)
	n = n*2; //Costo_fun++ (Costo_fun = 2)
	return n; //Costo_fun++ (Costo_fun = 3)
}

int main(){
	fun(5); //Costo += Costo_fun (Costo = 3)
	return 0; //Costo++ (Costo = 4)
}
//Totale: Costo = 4
```
- Istruzioni e condizioni con chiamate a funzioni hanno costo pari alla somma del costo delle funzioni invocate più 1;
```C
int fun(int n){
	n++; //Costo_fun++ (Costo_fun = 1)
	n = n*2; //Costo_fun++ (Costo_fun = 2)
	return n; //Costo_fun++ (Costo_fun = 3)
}

//(Costo = 0)
int main(){
	int var = fun(5); //Costo += Costo_fun + 1 (Costo = 4)
	return 0; //Costo++ (Costo = 5)
}
//Totale: Costo = 5
```

## ANALISI DEL CASO PEGGIORE
Quando si vuole calcolare il caso peggiore si deve trovare una **Funzione di Complessità**, come nell'esempio riportato in basso.

Nei cicli: 
- Se la variabile di controllo parte da $0$ allora il controllo si esegue $n+1$ volte. Tutte le istruzioni all'interno del corpo del ciclo si eseguono $n$ volte;
- Se la variabile di controllo parte da $k$ allora il controllo si esegue $(n+1)-k$ volte. Tutte le istruzioni all'interno del corpo del ciclo si eseguono $n-k$ volte.

<font color="orange">Esempio</font>: Calcolare la complessità nel caso peggiore del seguente algoritmo di ricerca in un array (dove `k` non si trova nell'array).
```C
int ricerca(int v[], int size, int k){
	int i;
	for(i = 0; i < size; i++)
		if (v[i] == k)
			return i;
	return -1;
}
```

- `int i` e passaggio argomenti: costo nullo;
- `i = 0`: il costo aumenta di 1 (costo = $1$);
- `i < size`: il controllo si esegue sempre $n+1$ volte, dato che la variabile `i` comincia da 0 (costo = $n+2$);
- `i++`: questa istruzione si esegue ad ogni iterata, ovvero $n$ volte (costo = $2n+2$);
- `if (v[i] == k)`: questo controllo si esegue ad ogni iterata, ovvero $n$ volte (costo = $3n+2$);
- `return i`: non si esegue mai, dato che `k` non si trova nell'array;
- `return -1`: il costo aumenta di 1 (costo = $\color{#CC241D}3n+3$).

## COMPORTAMENTO ASINTOTICO
Nell'analizzare la complessità di tempo di un algoritmo siamo interessati a come aumenta il tempo di completamento al crescere della taglia $n$ dell'input.

Infatti, per valori "piccoli" di $n$ il tempo richiesto di quasi tutti gli algoritmi è basso.
Quindi vogliamo sapere il comportamento dell'algoritmo quando $n$ diventa "*molto grande*".



Suddividiamo tutti gli algoritmi in **Classi di Complessità**, dove il primo riportato è il migliore e l'ultimo è il peggiore:
$$\textcolor{#61AFEF}1<<\textcolor{#61AFEF}{\sqrt{n}}<<\textcolor{#61AFEF}{log(n)}<<\textcolor{#61AFEF}n<<\textcolor{#61AFEF}{n\cdot log(n)}<<\textcolor{#61AFEF}{n^2}<<\textcolor{#61AFEF}{n^3}<<\textcolor{#61AFEF}{2^n}$$

![[Pasted image 20220424115723.png]]

In questo caso l'asse delle ordinate ($y$) rappresenta il tempo necessario al completamento del compito assegnato ad un algoritmo, mentre l'asse delle ascisse ($x$) rappresenta il numero di input $n$.

- La classe più veloce è la costante $1$, ma questo tipo si incontra raramente negli algoritmi.

- La seconda classe più veloce è il logaritmo $log_2(n)$, perché è la classe che tende a restare più vicino all'asse delle ascisse al crescere di $n$.

- La classe più lenta è l'esponenziale $2^n$, dato che la funzione aumenta subito di molto, anche se $n$ è relativamente piccolo.

___
[[000 Indice PSD|↖ Ritorna all'indice ↖]]

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