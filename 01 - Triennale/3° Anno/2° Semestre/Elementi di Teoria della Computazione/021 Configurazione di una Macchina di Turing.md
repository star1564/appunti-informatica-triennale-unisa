---
aliases:
  - Computazione di una Macchina di Turing
  - Passo di Computazione
  - Configurazione Iniziale
  - Configurazione di Arresto
  - Configurazione di Accettazione
  - Configurazione di Rifiuto
tags:
  - corsi/informatica/elementi_teoria_computazione
paragrafo: Macchine di Turing
cssclasses:
  - 
---
Durante la [[019 Funzione di Transizione delle Macchine di Turing#Definizione Informale della Computazione di una Macchina di Turing|Computazione]] di una Macchina di Turing, si verificano cambiamenti su:
- Lo *stato corrente* - rappresenta lo stato in cui la macchina si trova durante l'esecuzione
- La *posizione della testina* - indica la posizione della testina di lettura/scrittura sul nastro
- Il *contenuto all'inizio del nastro* - comprende una porzione iniziale (generalmente limitata a un numero finito di celle) contenente caratteri diversi dal blank, eventualmente separati da un blank

>Un'impostazione di questi tre elementi è chiamata una **configurazione** della Macchina di Turing.
>
>Quindi, una configurazione di una Macchina di Turing è una *snapshot presa ad un dato istante*, costituita da una porzione "abbastanza lunga" del nastro, dalla posizione della testina e dallo stato corrente.

> [!quote] Definizione Formale
>Una **configurazione** $\color{#CC241D}C$ di una Macchina di Turing $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ è una stringa $$C=\textcolor{#e86162}u\textcolor{#00C575}q\textcolor{#fdaa39}v\in\textcolor{#e86162}{\Gamma^*}\cdot \textcolor{#00C575}Q\cdot \textcolor{#fdaa39}{\Gamma^S}$$
>dove:
>- $\textcolor{#00C575}q\in Q$ è lo *stato corrente* di $M$
>- $\textcolor{#fdaa39}{\Gamma^S}=\Gamma^*\cdot(\Gamma-\{\sqcup\})\cup\{\sqcup,\epsilon\}$, ovvero che dopo l'*ultimo simbolo* di $\textcolor{#fdaa39}v$, il nastro *contiene solo simboli blank*
>- $\textcolor{#e86162}u,\textcolor{#fdaa39}v\in\Gamma^*$ è il *contenuto del nastro*, con la convenzione di aver eliminato tutti i simboli $\sqcup$ che seguono: 
>	- l'ultimo carattere di $\textcolor{#fdaa39}v$ se $v\neq\epsilon$ 
>	- $\textcolor{#e86162}u$ altrimenti
>- la *testina* è posizionata:
>	- sul primo simbolo di $\textcolor{#fdaa39}v$ se $v\neq\epsilon$ 
>	- su $\sqcup$ altrimenti
>- $\color{#e86162}u$ è la stringa di caratteri a sinistra del simbolo su cui è posizionata la testina
>- $\color{#e86162}u$ e $\color{#fdaa39}v$ possono essere vuote ($\epsilon$)

^fa0644

> [!example]+ <font color="orange">Esempio</font>
>$$\textcolor{#e86162}{1011}\textcolor{#00C575}{q_7}\textcolor{#fdaa39}{01111}$$
>Questa rappresenta la configurazione in cui:
>- Il nastro contiene $\textcolor{#e86162}{1011}\color{#fdaa39}01111\sqcup\sqcup\sqcup\dots$
>- Lo stato corrente è $\textcolor{#00C575}{q_7}$
>- La testina è posizionata sul secondo $0$, all'inizio di $\color{#fdaa39}v$
>
>![[Pasted image 20240429115154.png|600]]

> [!tip]- Casi particolari di configurazioni
>- Se $C=\textcolor{#00C575}q\textcolor{#fdaa39}v$ è una configurazione di una Macchina di Turing, allora la testina è posizionata *sulla prima cella del nastro*
>
>![[Pasted image 20240429115300.png|400]]
>
>- Se $C=\textcolor{#e86162}u\textcolor{#00C575}q$ è una configurazione di una Macchina di Turing, allora la testina è posizionata *sulla prima cella della porzione del nastro contenente solo $\sqcup$*
>
>![[Pasted image 20240429115427.png|400]]

## Passo di Computazione
>Si dice che la configurazione $C_1$ **produce** la configurazione $C_2$ se la Macchina di Turing *può passare da $C_1$ a $C_2$* in un **unico passo** per effetto della [[019 Funzione di Transizione delle Macchine di Turing|funzione di transizione]].
>
>Se questo avviene si scrive che $$C_1\to C_2$$
>dove la trasformazione $\to$ di $C_1$ in $C_2$ prende il nome di **passo di computazione**.

### Spostamenti a sinistra e a destra
Sia $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ una Macchina di Turing deterministica.

Siano due configurazioni di $M$ formate da: 
- $\textcolor{#00C575}{q_i},\textcolor{#00C575}{q_j}\in Q$
- $\textcolor{#FF6611}a,\textcolor{#FF6611}b,\textcolor{#FF6611}c\in\Gamma$
- $\textcolor{#61AFEF}u,\textcolor{#61AFEF}v\in\Gamma^*$

#### Sinistra
Nel caso in cui la Macchina di Turing effettua uno **spostamento verso sinistra**, diremo che 
$$
\textcolor{#61AFEF}u\textcolor{#FF6611}a\ \textcolor{#00C575}{q_i}\ \textcolor{#FF6611}b\textcolor{#61AFEF}v
\text{ produce }
\textcolor{#61AFEF}u\ \textcolor{#00C575}{q_j}\ \textcolor{#FF6611}a\textcolor{#FF6611}c\textcolor{#61AFEF}v
$$
se nella funzione di transizione $\delta(\textcolor{#00C575}{q_i},\textcolor{#FF6611}b)=(\textcolor{#00C575}{q_j},\textcolor{#FF6611}c,L)$.

#### Destra
Nel caso in cui la Macchina di Turing effettua uno **spostamento verso destra**, diremo che 
$$
\textcolor{#61AFEF}u\textcolor{#FF6611}a\ \textcolor{#00C575}{q_i}\ \textcolor{#FF6611}b\textcolor{#61AFEF}v
\text{ produce }
\textcolor{#61AFEF}u\textcolor{#FF6611}a\textcolor{#FF6611}c\ \textcolor{#00C575}{q_j}\ \textcolor{#61AFEF}v
$$
se nella funzione di transizione $\delta(\textcolor{#00C575}{q_i},\textcolor{#FF6611}b)=(\textcolor{#00C575}{q_j},\textcolor{#FF6611}c,R)$.

#### Casi particolari di configurazioni
 Per $C=qv$ (estremità sinistra) abbiamo che: 
 - $q_i\ bv\text{ produce } q_j\ cv$ se $\delta(q_i,b)=(q_j,c,L)$
 - $q_i\ bv\text{ produce } c\ q_j\ v$ se $\delta(q_i,b)=(q_j,c,R)$

## Tipi di Configurazioni
Una configurazione $C$ di una Macchina di Turing $M$, si dirama in diversi tipi.

| Nome                                            |                                                                        |                                                                                                                                        |
| ----------------------------------------------- | ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Configurazione Iniziale** di $M$ su input $w$ | $$C=q_0\ w,\quad w\in\Sigma^*$$                                        | È la configurazione che indica che *la macchina è nello stato iniziale* $q_0$ con la testina nella posizione più a sinistra sul nastro |
| **Configurazione di Accettazione**              | $$C=u\ q_{accept}\ v,\quad u,v\in\Gamma^*$$                            | Lo stato della configurazione è quello di *accettazione* ($q_{accept}$)                                                                |
| **Configurazione di Rifiuto**                   | $$C=u\ q_{reject}\ v,\quad u,v\in\Gamma^*$$                            | Lo stato della configurazione è quello di *rifiuto* ($q_{reject}$)                                                                     |
| **Configurazione di Arresto (*categoria*)**     | $$C=u\ q\ v,\quad u,v\in\Gamma^*,\quad q\in\{q_{accept},q_{reject}\}$$ | Le configurazioni di accettazione e di rifiuto rientrano in questa categoria e *non producono ulteriori configurazioni*                |

^f0bd9d

## Computazione su un input
> [!quote] Definizione Formale con Arresto
>Siano $C$ e $C'$ due configurazioni.
>
>Una Macchina di Turing *termina la computazione* sull'input $w\in\Sigma^*$ se esiste una [[026 Sequenza|sequenza]] di $k\geq 1$ configurazioni $C_1,C_2,\dots,C_k$ tale che:
>1. $C_1=C$, ovvero che $C_1$ è la configurazione *iniziale* di $M$ su input $w$
>2. $C_i\to C_{i+1}, \forall i\in\{1,\dots,k-1\}$, ovvero che *ogni $C_i$ produce $C_{i+1}$*
>3. $C_k=C'$, ovvero che $C_k$ sia una configurazione di *arresto*
>
>Diremo che è una **computazione** di lunghezza $k-1$ e si scrive che
>$$C\textcolor{#CC241D}{\to^*}C'$$

^d7a838

> [!tip]- Differenza tra Passo Computazionale e Computazione
> Una computazione è una *sequenza di [[#Passo di Computazione|passi di computazione]]* che partono dalla configurazione iniziale $C$ e terminano alla configurazione di arresto $C'$.
> Invece un passo di computazione è il singolo passaggio da una computazione ad un'altra.
> 
> ![[Pasted image 20240429153913.png|600]]

# 

___
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