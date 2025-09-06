---
aliases:
    - RE
    - Teorema di Kleene
    - espressione regolare
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Espressioni Regolari
cssclasses:
    - 
---

> Le **Espressioni Regolari** (Regular Expressions, _RE_) sono delle espressioni che _descrivono (o denotano) [[001 Linguaggio|linguaggi]]_. Sono composte da _operazioni regolari_ che vengono usate proprio come nelle espressioni aritmetiche normali.

> [!info] Teorema di Kleene
> Un linguaggio è [[007 Linguaggio Regolare|regolare]] se e solo se esiste un'espressione regolare che lo rappresenta.

> [!quote] Definizione Ricorsiva di Espressione Regolare
> Supponiamo di avere un alfabeto $\Sigma$. Le **Espressioni Regolari** vengono descritte ricorsivamente come segue:
>
> -   **Passo Base**: $\forall a\in\Sigma$ 
> 	$$\color{#61AFEF}a\text{ è una espressione regolare,}$$ 
> 	$$\color{#61AFEF}\epsilon\text{ è una espressione regolare,}$$ 
> 	$$\color{#61AFEF}\emptyset\text{ è una espressione regolare}$$
> -   **Passo Ricorsivo**: Se $E_1,E_2$ sono espressioni regolari, allora
>     $$\color{#61AFEF}(E_1)\text{ è una espressione regolare,}$$ 
>     $$\color{#61AFEF}(E_1\cup E_2)\text{ è una espressione regolare,}$$ 
>     $$\color{#61AFEF}(E_1E_2)=(E_1\circ E_2)\text{ è una espressione regolare,}$$ 
>     $$\color{#61AFEF}(E_1^*)\text{ è una espressione regolare}$$

^308157

Tutte le operazioni regolari hanno una certa precedenza rispetto alle altre:

| Operatore | Precedenza |
| :-------: | :--------: |
|    $*$    |     1      |
|  $\circ$  |     2      |
|  $\cup$   |     3      |

Quindi $ab^*\cup b$ corrisponde a $(a(b)^*)\cup b$.

> [!quote] Definizione Ricorsiva dei linguaggi rappresentati (o descritti) dalle espressioni regolari
> Data un'espressione regolare $E$ su un alfabeto $\Sigma$, indicheremo con $\mathcal{L}(E)$ il linguaggio che essa rappresenta, definito come segue:
>
> -   **Passo Base**: $\forall a\in\Sigma$ 
> 	$$\color{#61AFEF}\mathcal{L}(a)=\{a\}$$ 
> 	$$\color{#61AFEF}\mathcal{L}(\epsilon)=\{\epsilon\}$$ 
> 	$$\color{#61AFEF}\mathcal{L}(\emptyset)=\emptyset$$
> -   **Passo Ricorsivo**: Se $E_1,E_2$ sono espressioni regolari, allora
>     $$\color{#61AFEF}\mathcal{L}((E_1))=\mathcal{L}(E_1)\text{, ovvero che le parentesi possono essere omesse}$$ 
>     $$\color{#61AFEF}\mathcal{L}(E_1\cup E_2) = \mathcal{L}(E_1) \cup \mathcal{L}(E_2)$$ 
>     $$\color{#61AFEF}\mathcal{L}(E_1E_2)=\mathcal{L}(E_1)\mathcal{L}(E_2)=\mathcal{L}(E_1)\circ \mathcal{L}(E_2)=\mathcal{L}(E_1\circ E_2)$$ 
>     $$\color{#61AFEF}\mathcal{L}(E_1^*)=\mathcal{L}(E_1)^*$$

---

Con $\Sigma=\{a,b\}$
Non contiene $aa$: $(b\cup ab)^*(a\cup\epsilon)$
Contiene esattamente 1 occorrenza di $aa$: $(b\cup ab)^*aa(b\cup ba)^*$
Contiene al massimo una occorrenza di $aa$ come fattore: contiene esattamente 1 occorrenza di $aa$.

> [!example]- <font color="orange">Esempio</font>
> La seguente è una espressione regolare
> $$E=(a\cup b)^*$$
> È una combinazione di caratteri $a$ e $b$. Infatti:
> $$\mathcal{L}(E)=\mathcal{L}((a\cup b)^*)=(\mathcal{L}(a\cup b))^*=(\mathcal{L}(a)\cup \mathcal{L}(b))^*=(\{a\}\cup \{b\})^*=\color{#CC241D}\{a,b\}^*$$
>
> ---
>
> La seguente è una espressione regolare
> $$E=(0\cup1)0^*$$
>
> Infatti, il valore di $E$ è il linguaggio che consiste di tutte le stringhe che iniziano con uno 0 o un 1 seguito da un qualsiasi numero di simboli uguali a 0.
> Otteniamo questo risultato dividendo l'espressione nelle sue parti.
>
> $$\mathcal{L}(E)=\mathcal{L}((0\cup1)0^*)=(\mathcal{L}(0\cup1)\circ \mathcal{L}(0^*))=((\mathcal{L}(0)\cup \mathcal{L}(1))\circ(\mathcal{L}(0))^*)=(\{0\}\cup\{1\})\circ(\{0\}^*)=\color{#CC241D}\{0,1\}\circ\{0\}^*$$


## Proprietà delle Espressioni Regolari

### Unione

Le Espressioni Regolari seguono le stesse proprietà degli insiemi per l'[[008 Unione tra Insiemi#PROPRIETÀ|unione]], quindi:

-   $E_1\cup E_2=E_2\cup E_1$
-   $(E_1\cup E_2)\cup E_3=E_1\cup (E_2 \cup E_3)$
-   $\emptyset\cup E_1=E_1\cup \emptyset=E_1$
-   $E_1 \cup E_1 = E_1$

Inoltre:

-   $E_1(E_2 \cup E_3) = E_1E_2 \cup E_1 E_3$
-   $(E_1 \cup E_2) E_3 = E_1 E_3 \cup E_2 E_3$
-   $((E_1^*))^* = E_1^*$
-   $E^+ = EE^*$, ovvero che la [[003 Chiusura di Kleene#Chiusura Positiva di Kleene|Chiusura Positiva]] equivale alla concatenazione dell'espressione con la sua star
-   $(E_1\cup E_2)^*=(E_1^*E_2^*)^*$

### Concatenazione

-   $(E_1E_2)E_3=E_1(E_2E_3)$
-   $\emptyset E_1=E_1\emptyset =\emptyset$
-   $\epsilon E_1=E_1\epsilon =E_1$

---

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
