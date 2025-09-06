---
aliases:
  - Precision
  - Recall
  - F1 Score
  - Accuracy
  - Metrics-Level
  - ROC curve
  - Data-Level
  - Sottocampionamento
  - Sovracampionamento
  - Apprendimento a due fasi
  - Campionamento dinamico
  - Algorithm-Level
  - Cost-Sensitive Learning
  - Class-Balanced Loss
  - Focal Loss
tags:
  - corsi/informatica/machine_learning
paragrafo: Training Data
cssclasses:
  - 
---
Esistono diverse strategie per bilanciare il dataset. Anche se queste non sono delle soluzioni definitive possono mitigare di molto gli effetti dello sbilanciamento:
- Strategie *metrics-level*;
- Strategie *data-level*;
- Strategie *algorithm-level*.

## Strategie Metrics-Level
>La strategia di **Metrics-Level** si basa sulla *scelta di metriche appropriate* per la gestione e la valutazione dello sbilanciamento dei dati. La scelta di queste metriche è uno dei passi fondamentali quando si ha a che fare con un dataset sbilanciato.

In una semplice [[007 Task di Classificazione#Classificazione Binaria|Classificazione Binaria]] (con due classi), ci concentriamo sul valutare le *prestazioni del modello rispetto a una delle due classi*, che identifichiamo come "positiva". Questo approccio è comune quando si tratta di misurare la capacità del modello di identificare correttamente gli esempi di interesse. Le metriche di valutazione utilizzate comunemente includono Precision, Recall e F1 Score.

![[Pasted image 20231219104239.png|700]]

La **Precision** indica la percentuale di predizioni corrette di classe positiva rispetto al totale delle predizioni di classe positiva.
$$\text{Precision} = \frac{TP}{TP+FP}$$ ^348afa

La **Recall** indica la percentuale di predizioni corrette di classe positiva rispetto al totale dei casi di classe positiva. È anche chiamata *True Positive Rate*.
$$\text{Recall} = \frac{TP}{TP+FN}$$

L'**Accuracy** è una misura più generale sulle performance del modello. Essa indica il numero di volte che il modello ha classificato correttamente rispetto al numero totale di occorrenze da classificare. 
$$\text{Accuracy} = \frac{TP+TN}{TP+TN+FP+FN}$$ ^363748

L'**F1 Score** combina la Precision e la Recall per fornire una misura complessiva delle prestazioni.
$$\text{F1 Score} = 2\cdot\frac{\text{recall}\cdot\text{precision}}{\text{recall}+\text{precision}}$$ ^7c5678

> [!failure] Accuracy ed Error Rate
> Queste sono le metriche più usate, ma *non sono adatte* in situazioni di dataset sbilanciati, perché trattano tutte le classi equamente, quindi le classi maggioritarie domineranno queste metriche.

È importante notare che queste metriche sono asimmetriche, il che significa che il loro valore dipende da quale delle due classi viene considerata "positiva". La scelta della classe positiva può influenzare significativamente l'interpretazione e la valutazione delle performance del modello.

> [!example]- <font color="orange">Esempio</font>
>**Confusion Matrix per un classificatore binario:**
>
>|                          |  Predicted Positive  |  Predicted Negative  |
>|:-------------------------|:---------------------|:---------------------|
>|  <b>Actual Positive</b>  |                   45 ($\textcolor{#00C575}{TP}$) |                   10 ($\textcolor{#FF6611}{FN}$) |
>|  <b>Actual Negative</b>  |                   22 ($\textcolor{#CC241D}{FP}$) |                   90 ($\textcolor{#61AFEF}{TN}$) |  
>
>$\text{Precision} = \frac{45}{45+22}= 0,6716$
>$\text{Recall} = \frac{45}{45+10}= 0,8182$
>$\text{Accuracy} = \frac{45+90}{45+90+22+10}= 0,8084$
>$\text{F1-Score} = \frac{2 \cdot\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}= 0,7377$
>
>**Confusion Matrix per un classificatore multiclasse:**
>
>|                       | Predicted Class A         | Predicted Class B         | Predicted Class C         |
>|:--------------------- |:------------------------- |:------------------------- |:------------------------- |
>| <b>Actual Class A</b> | 66 ($\textcolor{#00C575}{TP_A},\textcolor{#61AFEF}{TN_{B}},\textcolor{#61AFEF}{TN_{C}}$) | 20 ($\textcolor{#FF6611}{FN_{A}},\textcolor{#CC241D}{FP_B},\textcolor{#61AFEF}{TN_{C}}$) | 0 ($\textcolor{#FF6611}{FN_{A}},\textcolor{#61AFEF}{TN_B},\textcolor{#CC241D}{FP_{C}}$)  |
>| <b>Actual Class B</b> | 0 ($\textcolor{#CC241D}{FP_{A}},\textcolor{#FF6611}{FN_B},\textcolor{#61AFEF}{TN_{C}}$)  | 34 ($\textcolor{#61AFEF}{TN_{A}},\textcolor{#00C575}{TP_B},\textcolor{#61AFEF}{TN_{C}}$) | 9 ($\textcolor{#61AFEF}{TN_{A}},\textcolor{#FF6611}{FN_B},\textcolor{#CC241D}{FP_{C}}$)  |
>| <b>Actual Class C</b> | 1 ($\textcolor{#CC241D}{FP_{A}},\textcolor{#61AFEF}{TN_B},\textcolor{#FF6611}{FN_{C}}$)  | 0 ($\textcolor{#61AFEF}{TN_{A}},\textcolor{#CC241D}{FP_B},\textcolor{#FF6611}{FN_{C}}$)  | 88 ($\textcolor{#61AFEF}{TN_{A}},\textcolor{#61AFEF}{TN_{B}},\textcolor{#00C575}{TP_C}$) |
>
>$\text{Overall Accuracy} = \frac{\text{Predizioni Corrette}}{\text{Numero Istanze}} = \frac{188}{188+9+20+1}=86,24\%$
>
>$\text{Precision}_{A} = \frac{TP_{A}}{TP_{A} + FP_{A}} = \frac{66}{66+(0+1)}= 0,99$
>$\text{Precision}_{B} = \frac{TP_{B}}{TP_{B} + FP_{B}} = \frac{34}{34+(20+0)}= 0,63$
>$\text{Precision}_{C} = \frac{TP_{C}}{TP_{C} + FP_{C}} = \frac{88}{88+(0+9)}= 0,91$
>
>$\text{Recall}_{A} = \frac{TP_{A}}{TP_{A} + FN_{A}} = \frac{66}{66+(20+0)}= 0,77$
>$\text{Recall}_{B} = \frac{TP_{B}}{TP_{B} + FN_{B}} = \frac{34}{34+(0+9)}= 0,79$
>$\text{Recall}_{C} = \frac{TP_{C}}{TP_{C} + FN_{C}} = \frac{88}{88+(1+0)}= 0,99$
>
>$\text{F1-Score}_{A} = \frac{2 \cdot\text{Precision}_{A} \cdot \text{Recall}_{A}}{\text{Precision}_{A} + \text{Recall}_{A}}= 0,86$
>$\text{F1-Score}_{B} = \frac{2 \cdot\text{Precision}_{B} \cdot \text{Recall}_{B}}{\text{Precision}_{B} + \text{Recall}_{B}}= 0,70$
>$\text{F1-Score}_{C} = \frac{2 \cdot\text{Precision}_{C} \cdot \text{Recall}_{C}}{\text{Precision}_{C} + \text{Recall}_{C}}= 0,95$

### ROC Curve
Molte attività di classificazione *possono essere trattate come problemi di regressione*, dove il modello restituisce una probabilità per ciascun esempio. Introduciamo una soglia per classificare gli esempi come positivi o negativi, consentendoci di influenzare le metriche di valutazione. Ad esempio, possiamo regolare la soglia per aumentare la Recall a scapito dei falsi positivi.

L'effetto di diverse soglie può essere visualizzato tramite una **ROC curve** (*Receiver Operating Characteristic*). La ROC Curve si concentra sulla classe positiva, aiutandoci a valutare le prestazioni del modello in vari scenari di classificazione.

![[Pasted image 20231215205208.png|500]]

Quando la Recall è 1, il modello è perfetto. Possiamo calcolare l'*area sotto la curva (AUC)*. Più ci avviciniamo alla perfezione maggiore sarà la grandezza di quest'area.

## Strategie Data-Level
>La strategia di **Data-Level** si interviene direttamente sui dati, *modificandone la [[024 Funzione di Distribuzione|distribuzione]] per il training*, in modo tale da ridurre il livello di sbilanciamento favorendo l'apprendimento del modello.

La strategia più comune è quella di **Ricampionamento**. Ovvero una tecniche che va ad *aggiungere o rimuovere campioni* dal dataset per bilanciarlo.

![[Pasted image 20231215205802.png|600]]

Il ricampionamento può essere di vari tipi.

### Sottocampionamento
>La tecnica del **Sottocampionamento** va a rimuovere campioni dalla classe maggioritaria, in modo tale da *ridurre la differenza con quella minoritaria*. Si ottiene ciò *rimuovendo a caso* dei campioni.

Esiste il rischio di *perdere dati importanti*.

Il metodo più popolare di questo tipo si chiama **Tomek**, utilizzato per il trattamento di dati a bassa dimensionalità:
- Si generano *coppie di campioni* di classi opposte sulla base della loro vicinanza;
- Si rimuove il campione della classe maggioritaria per ogni coppia.

![[Pasted image 20231215210309.png]]


### Sovracampionamento
>La tecnica del **Sovracampionamento** va ad aggiungere campioni della classe minoritaria in modo tale da *ridurre la differenza con quella maggioritaria*. Si ottiene ciò *generando* coppie di campioni minoritari in modo randomico.

Esiste il rischio di avere *[[Overfitting]] del modello*.

Il metodo più popolare di questo tipo si chiama **SMOTE**, utilizzato per il trattamento di dati a bassa dimensionalità. Si sintetizzano *nuovi campioni* della classe minoritaria. Questa sintesi avviene campionando combinazioni convesse di data point esistenti all'interno della classe minoritaria.

![[Pasted image 20231215210636.png|800]]

### Apprendimento a due fasi
>Nella tecnica dell'**Apprendimento a due fasi** si eseguono tre fasi: 
>1. Si fa il [[#Sottocampionamento|sottocampionamento]] del dataset fino ad avere $N$ campioni per ogni classe;
>2. Si addestra il modello con il dataset ricampionato;
>3. Si fa il [[Fine-Tuning]] del modello utilizzando il dataset originale.

### Campionamento dinamico
>Nella tecnica del **Campionamento Dinamico**, durante l'addestramento del modello, si combina il sovracampionamento ed il sottocampionamento:
>1. Si fa il [[#Sovracampionamento|sovracampionamento]] delle classi meno performanti;
>2. Si fa il [[#Sottocampionamento|sottocampionamento]] delle classi più performanti.

Questa tecnica ci permette di migliorare il modello, in quando *gli viene mostrata meno volte* la classe che ha *già imparato* a predirre. Di conseguenza, gli vengono *mostrati più campioni* della classe su cui ha ancora difficoltà.

## Strategie Algorithm-Level
>La strategia di **Algorithm-Level** apporta delle *modifiche a livello di algoritmo d'apprendimento* per mitigare l'effetto dello sbilanciamento. Questa strategia rende il modello più robusto allo sbilanciamento del dataset.

Ci sono tre tipi di metodi per questa strategia. Queste lavorano generalmente sulla [[009 Funzioni Obbiettivo (ML)|Loss Function]], dando priorità per la modifica alle predizioni che producono una *perdita maggiore*.

### Cost-Sensitive Learning
>Nel metodo del **Cost-Sensitive Learning**, si sfrutta il fatto che commettere *errori di classificazione per diverse classi* può comportare *costi diversi*. Questo significa che gli errori di classificazione *non sono uniformemente penalizzati*, ma il loro *impatto finanziario o pratico* può variare a seconda della classe coinvolta.

La loss function viene modificata in modo tale da *prendere in considerazione la variabilità dei costi*.

> [!example] <font color="orange">Esempio</font>
>In un'applicazione medica, classificare erroneamente una condizione grave potrebbe avere conseguenze più gravi rispetto a una classificazione errata di una condizione meno critica.

Pertanto, attribuire costi diversi a errori di classificazione può migliorare le prestazioni del modello in situazioni in cui alcune classi sono più critiche di altre.
### Class-Balanced Loss
>Il metodo del **Class-Balanced Loss** prevede una "*punizione*" per il modello quando produce previsioni errate *sulla classe minoritaria*. 

Questo si ottiene assegnando un peso ad ogni classe in mainera *inversamente proporzionale* al loro numero di campioni. Classi *minoritarie o rare* avranno un peso maggiore rispetto alle altre.

### Focal Loss
>Nel metodo del **Focal Loss**, è possibile avere campioni nel dataset che sono più semplici da classificare rispetto ad altri. Per questi campioni, il modello impara molto velocemente. Si spinge il modello a concentrarsi sui campioni difficili, ovvero quelli con una probabilità di correttezza di predizione bassa.

Si regola la loss function in modo tale da assegnare un peso maggiore a quei campioni su cui il modello ha maggiori difficoltà.

___
[[000 Indice ML|↖ Ritorna all'indice ↖]]

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