---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JSP
cssclasses:
  - 
---
>**JavaBean** è una semplice [[Classi|Classe]] Java che *contiene un insieme di proprietà* e [[Metodi]] di accesso/modifica per accedere e modificare queste proprietà. Le proprietà possono essere di diversi tipi, come interi, stringhe, oggetti personalizzati e così via.

In pratica, un "bean" incapsula molti [[Oggetti]] in un solo oggetto, in modo che si possa accedere a questi oggetti in più files.

L'utilizzo di JavaBeans consente di creare *componenti software riutilizzabili* che possono essere facilmente collegati insieme per creare applicazioni complesse. 

Questa classe deve seguire le seguenti convenzioni:
- Ha un [[Costruttore di Oggetti|Costruttore]] senza argomenti (e anche senza codice);
- Deve avere proprietà private;
- Deve essere [[025 Serializzazione|serializzabile]];
- Deve mettere a disposizione metodi pubblici [[4 Eclipse Help - Codice automatico (Eclipse)#^2f8785|setter e getter]] per le proprietà;
- Deve essere contenuta in un package chiamato `bean`. 

> [!example]- <font color="orange">Esempio</font>
>```Java
>package bean;
>import java.io.Serializable;
>
>public class Persona implements Serializable {
>    private String nome;
>    private String cognome;
>    private int eta;
>
>    public Persona() {}
>
>    public String getNome() {
>        return nome;
>    }
>
>    public void setNome(String nome) {
>        this.nome = nome;
>    }
>
>    public String getCognome() {
>        return cognome;
>    }
>
>    public void setCognome(String cognome) {
>        this.cognome = cognome;
>    }
>
>    public int getEta() {
>        return eta;
>    }
>
>    public void setEta(int eta) {
>        this.eta = eta;
>    }
>}
>```

---
### Azioni JSP
Queste sono le [[017 Azioni JSP|Azioni JSP]] per JavaBean:

> [!note] ***useBean***
> Consente di creare un'istanza di un oggetto [[018 JavaBean|JavaBean]] e di assegnarlo ad una variabile di scripting.
> `<jsp:useBean id="nomeBean" class="nomeClasse" scope="page|request|session|application" />`
> 
> Dove:
> - *id* è il nome con cui l'istanza del bean verrà indicata nel resto della pagina;
> - *class* è la classe java che definisce il bean;
> - *scope* definisce l'ambito di accessibilità e tempo di vita dell'oggetto:
> 	- `page` (default), la variabile JavaBean sarà accessibile solo all'interno della pagina JSP in cui è stata creata.
> 	- `request`, la variabile JavaBean sarà accessibile all'interno della richiesta [[003 HTTP|HTTP]] corrente e in tutte le pagine JSP incluse nella richiesta.
> 	- `session`, la variabile JavaBean sarà accessibile per l'intera sessione dell'utente e in tutte le richieste HTTP effettuate dall'utente nella sessione corrente.
> 	- `application`, la variabile JavaBean sarà accessibile per l'intera durata dell'applicazione web e in tutte le sessioni degli utenti.

^71c572

> [!note] ***getPropriety***
> Consente di ottenere una proprietà su un oggetto JavaBean.
> `<jsp:getProperty name="nomeBean" property="nomeProprietà" />`
>
> Dove:
> - *name* è il nome (id) del bean a cui si fa riferimento;
> - *property* è il nome della proprietà di cui si vuole leggere il valore.

> [!note] ***setPropriety***
> Consente di impostare una proprietà su un oggetto JavaBean.
> `<jsp:setProperty name="nomeBean" property="nomeProprietà" value="valoreProprietà"/>`

> [!example]- <font color="orange">Esempio</font>
>```HTML
><!DOCTYPE html>
><html>
><head>
>	<title>Esempio di utilizzo delle azioni JSP</title>
></head>
><body>
>	<h1>Esempio di utilizzo delle azioni JSP</h1>
>	
>	<!-- Creazione di un nuovo JavaBean e memorizzazione nello scope "request" -->
>	<jsp:useBean id="persona" class="bean.Persona" scope="request" />
>
>	<!-- Impostazione delle proprietà del JavaBean tramite l'azione setProperty -->
>	<jsp:setProperty name="persona" property="nome" value="Mario" />
>	<jsp:setProperty name="persona" property="cognome" value="Rossi" />
>	<jsp:setProperty name="persona" property="eta" value="30" />
>
>	<!-- Lettura delle proprietà del JavaBean tramite l'azione getProperty -->
>	<p>Nome: <jsp:getProperty name="persona" property="nome" /></p>
>	<p>Cognome: <jsp:getProperty name="persona" property="cognome" /></p>
>	<p>Età: <jsp:getProperty name="persona" property="eta" /></p>
></body>
></html>
>```



___
[[000 Indice TSW|↖ Ritorna all'indice ↖]]

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
- https://www.javatpoint.com/java-bean