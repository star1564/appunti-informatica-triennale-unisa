---
aliases:
    - Robustezza di una MdT
    - Dimostrare l'equivalenza tra due varianti di MdT
tags:
    - corsi/informatica/elementi_teoria_computazione
paragrafo: Varianti delle Macchine di Turing
cssclasses:
    - 
---

Esistono diverse **varianti** della definizione di Macchina di Turing deterministica.

Le principali varianti sono quelle:

-   [[026 Macchina di Turing Multinastro|multinastro]]
-   [[029 Macchina di Turing Non Deterministica|non deterministiche]]

> [!quote] Macchine equivalenti (robustezza)
> Due macchine sono **equivalenti** se _[[023 Linguaggio Turing-Riconoscibile|riconoscono]] lo stesso linguaggio o la stessa classe di linguaggi_.
> Infatti, le due principali varianti hanno lo stesso potere [[021 Configurazione di una Macchina di Turing#Computazione su un input|computazionale]] delle Macchina di Turing [[018 Macchine di Turing|deterministiche]] perché riconoscono la stessa classe di linguaggi.
>
> Chiamiamo "_robustezza_" questa invarianza ad alcune variazioni nella definizione.

^7ce7c4


## Dimostrare l'equivalenza tra due varianti

Siano $\mathcal{T}_1$ e $\mathcal{T}_2$ due famiglie di modelli computazionali. Per dimostrare che i modelli in $\mathcal{T}_1$ hanno lo stesso potere computazionale dei modelli in $\mathcal{T}_2$, occorre far vedere che _per ogni_ macchina $M_1\in\mathcal{T}_1$ esiste $M_2\in\mathcal{T}_2$ [[#^7ce7c4|equivalente]] ad $M_1$ e _viceversa_.

Però, alle macchine di $\mathcal{T}_1$ potrebbe corrispondere una rappresentazione delle [[021 Configurazione di una Macchina di Turing|configurazioni]] diversa dalla rappresentazione delle configurazioni delle macchine in $\mathcal{T}_2$.

Quindi, la dimostrazione che vogliamo ottenere consiste spesso nella dimostrazione che _per ogni_ $M_1$ esiste una $M_2$ capace di **simularla** (e viceversa), ovvero che _sono in corrispondenza_ le:

-   configurazioni iniziali
-   configurazioni intermedie
-   eventuali configurazioni di arresto

### Cambio della funzione di transizione

Per illustrare la robustezza del modello di macchina di Turing cerchiamo di variare il tipo di [[019 Funzione di Transizione delle Macchine di Turing|funzione di transizione]] consentito.

Nella nostra definizione, la funzione di transizione forza la testina a spostarsi verso sinistra o destra ad ogni passo. Infatti, la testina non può restare ferma.
Supponiamo di aver permesso alla macchina di Turing la capacità di _restare ferma_.

> [!quote] Definizione Formale
> Chiamiamo il nuovo insieme di Macchine di Turing con questa caratteristica $\color{#CC241D}\mathcal{T}_{(L,R,S)}$.
>
> Questo è l'insieme delle macchine $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})$ dove i suoi elementi sono definiti come in una Macchina di Turing deterministica, tranne per la sua funzione di transizione, che è definita nel seguente modo
> $$\delta:(Q-\{q_{accept},q_{reject}\})\times\Gamma\to Q\times\Gamma\times\{L,R,\textcolor{#61AFEF}{S}\}$$

Chiamiamo $\mathcal{T}_{(L,R)}$ l'insieme delle Macchina di Turing che abbiamo definito in [[018 Macchine di Turing#Definizione Formale|precedenza]].

Quindi, se la direzione specificata nella funzione di transizione $\delta$ è $S$ (**stay** o stop), la testina _si troverà sulla stessa cella su cui si trovava all'inizio della computazione_.

> [!info]+ Questo non fa riconoscere altri linguaggi
> La caratteristica di far rimanere ferma la testina **non permette** alle macchine di Turing di riconoscere ulteriori linguaggi, cioè _non aggiunge potere computazionale_ al modello scelto. In quanto siamo in grado di convertire una qualsiasi Macchina di Turing con la possibilità di "restar ferma" in una che non ha tale capacità.
>
> Lo facciamo costruendo una Macchina di Turing in cui sostituiamo la transizione "resta ferma" ($S$) con due transizioni, una che sposta la testina a destra e una che la riporta a sinistra.

#### Da (L,R) a (L,R,S)

I modelli in $\mathcal{T}_{(L,R)}$ **hanno lo stesso potere computazionale** dei modelli in $\mathcal{T}_{(L,R,S)}$.

Se $M=(Q,\Sigma, \Gamma,\textcolor{#61AFEF}{\delta},q_0,q_{accept}, q_{reject})\in\mathcal{T}_{(L,R,S)}$ definiamo $M' = (Q,\Sigma, \Gamma,\textcolor{#61AFEF}{\delta'},q_0,q_{accept}, q_{reject})\in\mathcal{T}_{(L,R)}$ con
$$\textcolor{#61AFEF}{\delta(q, \gamma) = \delta'(q, \gamma)}, \quad\forall (q, \gamma) \in (Q - \{q_{accept},q_{reject}\})\times\Gamma$$
Ovviamente $\color{#CC241D}\mathcal{L}(M') = \mathcal{L}(M)$.

#### Da (L,R,S) a (L,R)

Consideriamo $Q^c=\{q^c\ |\ (q,\gamma,S)\in\delta((Q-\{q_{accept},q_{reject})\times\Gamma\}\}$, ovvero tutte le situazioni in cui la macchina di Turing si trova in uno stato non terminante, legge un simbolo sul nastro e _rimane ferma_ nella stessa posizione del nastro.

Se $M=(Q,\Sigma, \Gamma,\delta,q_0,q_{accept}, q_{reject})\in\mathcal{T}_{(L,R,S)}$ definiamo $M' = (Q\cup Q^c,\Sigma, \Gamma,\delta',q_0,q_{accept}, q_{reject})\in\mathcal{T}_{(L,R)}$ dove:

-   se $\color{#61AFEF}\delta(q_i,\gamma)=(q_j,\gamma',d)$ con $d\in\{L,R\}$, allora $\color{#61AFEF}\delta'(q_i,\gamma)=\delta(q_i,\gamma)$
-   se $\color{#61AFEF}\delta(q_i,\gamma)=(q_j,\gamma',S)$, allora, per ogni $\eta \in\Gamma$, sono entrambe vere
    -   $\delta'(q_i,\gamma)=\delta(q_j^c,\gamma',R)$
    -   $\delta'(q_j^c,\eta)=\delta(q_j,\eta,L)$

> [!tip]+ Spiegazione
>
> -   Se abbiamo una transizione normale $L$ o $R$, la macchina $M'$ copia direttamente il comportamento di $M$
> -   Se abbiamo una transizione con "stay" $(S)$, la macchina $M$ fa due cose:
>     1. Crea un corrispondente stato non terminante che ha la direzione "stay" dall'insieme $Q^c$ $(q_j^c)$ nella nuova macchina $M'$
>     2. Definisce le transizioni per _simulare_ il comportamento della "stay":
>         - La nuova macchina sposta la testina a destra $(R)$ dopo aver letto il simbolo $\gamma$ ed entra nel nuovo stato non terminante $q_j^c$
>         - Dal nuovo stato non terminante $q_j^c$ la macchina sposta la testina a sinistra $(L)$ ed entra nello stato originale che avrebbe dovuto seguire la transizione di "stay" $(S)$

Anche in questo caso $\color{#CC241D}\mathcal{L}(M) = \mathcal{L}(M')$.

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
