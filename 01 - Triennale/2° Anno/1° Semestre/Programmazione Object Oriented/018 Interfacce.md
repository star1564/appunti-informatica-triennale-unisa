---
aliases:
  - Interfacce
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Caratteristiche della Programmazione ad Oggetti
cssclasses:
  - 
---
Un'**Interfaccia** è un altro modo per poter [[017 Programmazione Generica in Java|programmare in maniera generica]] in Java. 
Funziona molto similmente alle [[Keyword abstract|classi astratte]].

Un'interfaccia si usa per raggruppare metodi di diverse classi che hanno la stessa funzione. Per esempio dei veicoli:
```java
// Interfaccia
interface VehicleMetodi {
  public void honk(); // metodo interfaccia (non ha una implementazione)
  public void start(); // metodo interfaccia (non ha una implementazione)
}
```

Una [[Classi|Classe]] **implementa una interfaccia** se la dichiara in una clausola **`implements`**.

```Java
// Car "implementa" l'interfaccia VehicleMetodi
class Car implements VehicleMetodi{
	// Dato che questa classe implementa l'interfaccia, ci devono essere PER FORZA questi metodi
	
	public void honk(){
		System.out.println("Car sta suonando");
	}
	public void start(){
		System.out.println("Car è partita");
	}
}

// Truck "implementa" l'interfaccia VehicleMetodi
class Truck implements VehicleMetodi{
	// Dato che questa classe implementa l'interfaccia, ci devono essere PER FORZA questi metodi
	public void honk(){
		System.out.println("Truck sta suonando");
	}
	public void start(){
		System.out.println("Truck è partito");
	}
}

public class Main{
	public static void main(String[] args){
		Car c = new Car();
		Truck t = new Truck();

		c.start();
		c.honk();

		t.start();
		t.honk();
	}
}
```

Una volta implementata una interfaccia all'interno di una classe, se la prima ha metodi si devono per forza specificarli.

Per implementare **molteplici interfacce** si separano i loro nomi nell'implementazione.
```Java
public class NomeClasse implements NomeInterfaccia1, NomeInterfaccia2, /*...*/
{
	//variabili di esemplare
	//metodi
}
```

> [!note]- Note sulle interfacce
> - Le interfacce non possono creare oggetti propri;
> - I metodi all'interno delle interfacce non sono implementati;
> - Se una classe implementa una interfaccia e si ha una sua sottoclasse, quest'ultima implementa automaticamente l'interfaccia;
> - I metodi delle interfacce sono automaticamente pubblici;
> - Gli attributi delle interfacce sono automaticamente pubblici, static e final;
> - Un'interfaccia non può contenere un costruttore.

> [!question]- Perché usare una interfaccia?
> 1) Per una maggiore sicurezza: si possono nascondere certi dettagli e mostrare solo informazioni importanti dell'oggetto;
> 2) Java non supporta l'"ereditarietà multipla" (una classe può solo [[015 Ereditarietà|derivare]] da una superclasse). Questo può essere possibile con le interfacce, perché una classe può implementare interfacce multiple;
> 3) Per raggruppare diverse classi sotto una o più caratteristiche.

---

![[Interfaccia Funzionale]]

___
[[000 Indice POO|↖ Ritorna all'indice ↖]]

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

> [!error]- Nota per l'esame orale di La Torre
> Se ti viene chiesto di creare una interfaccia ed una classe che la implementa, NON usare `System.out.println()` nell'implementazione di uno dei metodi.
> Dice che "potrebbero esserci problemi", ma non trovo nulla a riguardo. Il codice funziona ma è meglio presentare il seguente:
>```Java
>public interface A{
>	public String metodo1();
>	public int metodo2();
>}
>public class B implements A{
>	public String metodo1(){
>		//System.out.println("NON USARE LA SYSOUT!")
>		return "Metodo 1";
>	}
>	public int metodo2(){
>		return 2;
>	}
>}
>```
