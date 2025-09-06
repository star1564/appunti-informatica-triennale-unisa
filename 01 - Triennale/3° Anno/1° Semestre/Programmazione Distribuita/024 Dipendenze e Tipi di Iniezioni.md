---
aliases: 
  - Dependency Injection
  - Dipendenza
  - Inversion of Control
tags:
  - corsi/informatica/programmazione_distribuita
paragrafo: Java Enterprise Edition
cssclasses:
  - 
---
## Dipendenza
Una classe `ClassA` è **dipendente** dalla classe `ClassB` e dalla classe `ClassC` se `ClassA` usa `ClassB` e `ClassC` come variabili.
 ```Java
 public class ClassA { 
 	ClassB b; // <-- Dipendenza
 	ClassC c; // <-- Dipendenza
 }
```

![[Pasted image 20231021182417.png|300]]

### Loosely Coupled
In genere, la `Classe1` e la `Classe2` sono *loosely coupled*, se la `Classe1` avrà un riferimento a un'interfaccia invece di un riferimento diretto alla `Classe2`.

```Java
public class Class1 {
    public IClass2 c2; // <-- loosely coupled
}
 
public interface IClass2 {}
 
public class Class2 implements IClass2 {}
```


## Dependency Injection
>Il **Dependency-Injection (DI)** è un design pattern che prevede l'iniezione (*inserimento*) delle dipendenze di un oggetto *da parte di un componente esterno*, invece che farle creare o gestire dall'oggetto stesso.

![[Pasted image 20231021182449.png|300]]

> [!example]+ <font color="orange">Esempio Dependency Injection</font>
>```Java
>public class Employee {
>	private Address address;
>	
>	public Employee() {
>		this.address = new Address("XYZ Street");
>	}
>}
>```

## Inversion of Control
>Il pattern **Inversion-of-Control (IoC)** è una estensione del DI dove, invece di essere il codice client a chiamare direttamente un componente o un servizio, è il framework o *il contenitore IoC che gestisce il ciclo di vita e le chiamate ai componenti*. Ovvero, l'applicazione delega il controllo dell'esecuzione al framework o al contenitore.

Questo può avvenire attraverso il:
- *constructor injection*, dove la dipendenza viene passata attraverso il costruttore;
- *setter injection*, dove la dipendenza viene passata attraverso un setter.


> [!example]+ <font color="orange">Esempio Inversion of Control</font>
>- IoC, constructor injection:
>```Java
>public class Employee {
>	private Address address;
>	
>	public Employee(Address address) {
>		this.address = address;
>	}
>}
>```
>
>- IoC, setter injection:
>```Java
>public class Employee {
>	private Address address;
>	
>	public void setAddress(Address address) {
>		this.address = address;
>	}
>}
>```


___
[[000 Indice PD|↖ Ritorna all'indice ↖]]

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

https://www.youtube.com/watch?v=USLwFGTZB4E