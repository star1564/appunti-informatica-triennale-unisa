---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: JavaScript
cssclasses:
  - 
---
>**AJAX** (*Asynchronous [[028 Basi per JavaScript|JavaScript]] and [[034 XML|XML]]*) è una tecnica che consente di inviare richieste e ricevere dati da un server in modo asincrono, *senza dover ricaricare completamente la pagina web*. AJAX sfrutta il potere di JavaScript, insieme a tecnologie come XML, JSON e HTML, per creare interazioni dinamiche tra l'utente e il server.

Viene spesso utilizzato per implementare funzionalità come il caricamento dinamico dei dati, la verifica dell'autenticazione, l'invio di form senza ricaricare la pagina e molto altro ancora.

> [!info] XML o JSON
> Nonostante il nome faccia riferimento ad XML, AJAX può funzionare con una varietà di formati dati, come [[036 JSON con AJAX#JSON|JSON]], che è diventato uno *standard molto comune* per lo scambio di dati tra il client e il server.

---
### Eventi a due livelli

AJAX utilizza gli *eventi e le callback* per gestire il flusso di lavoro asincrono. Ci sono due livelli di eventi:
- **Eventi locali**: che portano ad una modifica diretta del [[030 JS ed elementi HTML#^fdd485|DOM]] da parte di JavaScript, e quindi a cambiamenti *locali* della pagina;
- **Eventi remoti**: ottenuti tramite il ricaricamento della pagina, che viene modificata *lato server* in base ai parametri passati in GET o POST.

Il ricaricamento della pagina per rispondere a un'interazione con l'utente prende il nome di **Postback**.

![[Pasted image 20230617100013.png|500]]



---
### XHR
Con JavaScript, viene creata un'istanza di un oggetto *`XMLHttpRequest`* (*XHR*) che gestirà la comunicazione tra il browser e il server. L'oggetto XHR fornisce metodi e proprietà per inviare e ricevere dati in modo asincrono.

![[Pasted image 20230617100322.png|650]]

Utilizzando l'oggetto XHR, viene specificato l'URL a cui inviare la richiesta HTTP (come GET o POST) al server. È possibile includere parametri, dati o payload nella richiesta, come informazioni di form o dati in formato JSON.

In AJAX, si può verificare un *evento* determinato dall'interazione fra utente e pagina web. 

Questo evento comporta l'esecuzione di una funzione JavaScript in cui:
- Si *istanzia* un oggetto `XMLHttpRequest`;
- Si *configura* l'oggetto XHR, associandogli una funzione di *callback* ed altri parametri;
- Si effettua la chiamata asincrona al server.

Il server elabora la richiesta e risponde al client invocando la funzione di callback, che:
- Elabora il risultato;
- Aggiorna il DOM della pagina per mostrare i risultati dell'elaborazione *senza ricaricare completamente la pagina*.

#### Creazione XHR e metodi
Un oggetto XHR si crea in questo modo:
```js
var xhr = new XMLHttpRequest();
```

Ci sono diversi metodi supportati da tutti i browser:
- ***`open(method, uri, async)`***: ha lo scopo di inizializzare la richiesta da formulare al server. Parametri:
	- `method`: una stringa con `"get"` o `"post"`;
	- `uri`: una stringa che identifica la risorsa da ottenere;
	- `async`: valore booleano che, se impostato a `true`, effettua una richiesta asincrona.
- ***`setRequestHeader(nomeHeader, valore)`***: consente di impostare gli header HTTP della richiesta da inviare. 
	- Viene chiamata più volte, una per ogni header da impostare; 
	- È importante impostare l'header `"connection"` al valore **`"close"`**.
- ***`send(body)`***: consente di inviare la richiesta al server.

> [!example]- <font color="orange">Esempio</font>
>Prelevare dati con GET:
>```js
>var xhr = new XMLHttpRequest();
>xhr.open("get", "pagina.html?p1=v1&p2=v2", true);
>xhr.setRequestHeader("connection", "close");
>xhr.send(null);
>```
>
>Mandare dati con POST:
>```js
>var xhr = new XMLHttpRequest();
>xhr.open("post", "pagina.html", true);
>xhr.setRequestHeader("content-type", "x-www-form-urlencoded");
>xhr.setRequestHeader("connection", "close");
>xhr.send("p1=v1&p2=v2");
>```

#### Valori e proprietà di XHR
Lo stato e i risultati della richiesta vengono memorizzati all'interno dell'oggetto XHR durante la sua esecuzione.

Le proprietà comunemente supportate dai vari browser sono:
- **`status`**, insieme a **`statusText`**. Ovvero un valore intero corrispondente all'[[004 Richiesta e Risposta HTTP#^6cccca|esito della richiesta HTTP]] ed una descrizione testuale.

```js
if(xhr.status != 200)
	alert(xhr.statusText);
```

- **`readyState`** è una proprietà in sola lettura di tipo intero che consente di leggere in ogni momento lo stato della richiesta. Ammette 5 valori:
	- 0: *uninitialized*, l'oggetto esiste, ma non è stato ancora richiamato `open()`;
	- 1: *open*, è stato invocato il metodo `open()`, ma `send()` non ha ancora effettuato l'invio dati;
	- 2: *sent*, metodo `send()` è stato eseguito e ha effettuato la richiesta;
	- 3: *receiving*, la risposta ha cominciato ad arrivare;
	- 4: *loaded*, l'operazione è stata completata (UNICO STATO SUPPORTATO DA TUTTI I BROWSER).

- **`onreadystatechange`** è la funzione di *callback* che viene richiamata in modo asincrono ad ogni cambio di stato della proprietà. Si deve inserire prima del `send()`.

- **`responseText`**, una stinga che contiene il body della risposta HTTP, disponibile solo a interazione ultimata (ovvero `readyState == 4`).

> [!example] [Esempio](https://www.w3schools.com/xml/tryit.asp?filename=tryajax_first)

La funzione di **callback** in AJAX viene utilizzata per gestire la risposta ricevuta da una richiesta AJAX in modo asincrono. Quando si effettua una richiesta AJAX, viene aperta una connessione al server e si verifica l'avanzamento della richiesta utilizzando la proprietà `readyState`. La funzione di callback *viene invocata ad ogni variazione dello stato di avanzamento della richiesta* (`readyState`), consentendo di gestire le diverse fasi del processo di richiesta e risposta.

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