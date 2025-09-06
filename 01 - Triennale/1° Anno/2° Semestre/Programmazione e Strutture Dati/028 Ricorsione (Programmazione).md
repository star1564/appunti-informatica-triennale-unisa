---
aliases: 
tags:
  - corsi/informatica/programmazione_strutture_dati
paragrafo: Ricorsione
cssclasses:
  - 
---
La **Ricorsione** è il processo nel quale una funzione *richiama* direttamente o indirettamente *se stessa*.
I linguaggi che gestiscono la ricorsione, lo fanno mediante il [[027 Record di Attivazione|record di attivazione]].

Praticamente, risolvere un problema con un approccio ricorsivo comporta:
1. Identificare un "*Passo Base*" con una soluzione nota;
2. Esprimere la soluzione al caso generico $n$ in termini dello stesso problema in uno o più casi semplici ($n-1$, $n-2$, ecc.).

Di solito le funzioni ricorsive sono meno efficienti delle funzioni iterative, ma il codice è più "leggibile".

Gli algoritmi ricorsivi hanno, generalmente, una struttura di questo tipo:
```C
Algoritmo A(I){
	if(I è di "taglia piccola")
		return output;
	else { 
		// Chiamate ricorsive
		...
		A(I_1);
		...
		A(I_a);
		...
	}
}
```

Dove: 
- `I` è l'input iniziale all'algoritmo;
- `...` sono qualsiasi tipo di istruzioni;
- `A(I_1)...A(I_a)` sono le $a$ chiamate ricorsive dell'algoritmo $A$, rispettivamente sugli input $I_1,\dots,I_a$.

## ESEMPIO 1: FATTORIALE
La funzione fattoriale(n), denotato come $n!$, è definito per tutti gli interi $n\geq 0$ come:
- Passo base: $\quad n!=1,\quad$ se $n=0$;
- Passo Ricorsivo: $\quad n! = n\cdot (n-1)!,\quad$ se $n>0$.

```C
int fact(int n){
	if (n == 0) //Oppure (n <= 1)
		return 1;
	else
		return n * fact(n-1);
}

int main(void){
	int fz, z = 3;
	fz = fact(z); //Si calcola il fattoriale di 3
}
```

Diagramma della funzione ricorsiva del fattoriale:
![[Pasted image 20220415111801.png]]

Nello stack del record di attivazione si ha questa situazione:
![[Pasted image 20220415112233.png]]
![[Pasted image 20220415112404.png]]

## ESEMPIO 2: FIBONACCI
Un altro esempio per la ricorsione è la successione di Fibonacci (aurea). 
Questa è una successione di interi positivi in cui ciascun numero è la somma dei due precedenti, dove i primi due sono per definizione 1.

Definizione ricorsiva:
- Passo base: $\quad F_0=1,F_1=1$;
- Passo Ricorsivo: $\quad F_n=F_{n-1}+F_{n-2},\quad \forall n>1$.

```C
int fibonacci(int n){
	if (n==0 || n==1)
		return 1;
	else
		return fibonacci(n-1)+fibonacci(n-2);
}
```

Diagramma della funzione ricorsiva di Fibonacci (con $n=3$):
![[Pasted image 20220415113202.png]]

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