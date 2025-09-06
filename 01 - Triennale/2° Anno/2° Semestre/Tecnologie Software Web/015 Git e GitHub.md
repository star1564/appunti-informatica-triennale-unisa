---
aliases: 
tags:
  - corsi/informatica/tecnologie_software_web
paragrafo: Tools per lo sviluppo di Software
cssclasses:
  - 
---
>**Git** è un sistema per il controllo delle versioni dei file da linea di comando. Permette di *tenere traccia delle modifiche e delle versioni* appropriate al codice sorgente del software.

Una **Repository** contiene tutti i file di un progetto.
All'interno di essa, è possibile la creazione di molteplici **Branch** (*rami*) di sviluppo. Permettono di lavorare su parti differenti di un progetto senza impattare la versione live del programma.

![[Pasted image 20230309180216.png|500]]

Ogni programma ha un branch principale (*master* o *main*) che rappresenta la versione finalizzata attuale del progetto.
Altri branch, oltre quello principale, sono delle *versioni copiate (nuove o separate)* del branch main, utilizzate per vari scopi (come il fix di bug).

Per salvare modifiche effettuate su un qualsiasi branch, si deve fare il **Commit**. Quando lo si fa, si dovrebbe include un messaggio, spiegando i cambiamenti della versione nuova.

Quando il lavoro con un branch è completato, si fa il **Merge** del branch secondario con quello principale.

Si può usare [Git](https://git-scm.com/) come programma assestante in [[1000.L-SO Shell Linux|linea di comando]] oppure si può usare [GitHub](https://github.com) (altamente consigliato non disponibile ufficialmente in linux). Su Visual Studio Code esiste una sezione dedicata a Git (source control) utilizzata per fare commit.

> [!success] [Video Consigliato (Fireship)](https://www.youtube.com/watch?v=HkdAHXoRtos&t=43s)
> [Estensione Consigliata per VSC (grafo repository con interattività sui commit)](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)

> [!success] [Sito Consigliato per imparare Git in modo interattivo](https://learngitbranching.js.org)


## COMANDI E CONCETTI
### COMMIT
In Git, i **commit** sono uno dei concetti fondamentali. I commit rappresentano punti fissi nella cronologia di una repository che *registrano uno stato specifico* dei file e sono caratterizzati da:
- Un titolo;
- Un messaggio (o descrizione);
- Un codice hash univoco.

![[Pasted image 20230919080549.png|500]]
#### Comando di base
Il comando `git commit` viene utilizzato per registrare le modifiche apportate ai file nella repository Git. Dopo aver apportato le modifiche ai tuoi file, usa `git add` per prepararli al il commit (stage), poi esegui il comando `git commit` per registrare le modifiche.

```bash 
git add nome_del_file
git commit -m "Descrizione del commit"
```

Questo crea un nuovo commit nella cronologia della repository, registrando le modifiche apportate ai file nell'ultimo stato aggiunto.

---
### REVERT COMMIT
In Git, il **revert** di un commit è un processo utilizzato per *annullare le modifiche apportate in un commit specifico* senza eliminare il commit stesso. Questo è utile quando si desidera correggere errori o annullare modifiche indesiderate in una repository Git.

![[Pasted image 20230918175533.png|400]]
#### Comando di base

Il comando principale per eseguire il revert di un commit è `git revert`. Viene utilizzato in questo modo:

```bash
git revert <hash_del_commit>
```

Dove `<hash_del_commit>` è l'hash del commit che si desidera annullare.
#### Come funziona
> [!note] Effetto sui commit successivi
> È importante notare che il revert di un commit può influenzare i commit successivi al commit reverted.


1. `git revert` crea un nuovo commit che contiene le modifiche necessarie per annullare le modifiche apportate nel commit specificato. Questo nuovo commit viene aggiunto alla cronologia del repository.

2. Il commit originale non viene eliminato e rimane nella cronologia dei commit. La nuova revisione dell'archivio contiene sia il commit originale che il commit di revert.

3. La cronologia dei commit viene preservata, consentendo di tenere traccia delle modifiche e di mantenere la coerenza della storia del progetto.

> [!example]- <font color="orange">Esempio</font>
>Supponiamo di avere una cronologia dei commit come segue:
>
>![[Pasted image 20230918175927.png]]
>
>- Commit A
>- Commit B (da annullare)
>- Commit C
>
>Per annullare le modifiche apportate nel Commit B e tornare allo stato del Commit A, eseguiamo il seguente comando:
>
>```bash
>git revert <hash_del_commit_B>
>```
>
>Questo creerà un nuovo commit che annulla le modifiche di Commit B (e implicitamente anche quelle di Commit C), risultando in:
>
>- Commit A
>- Commit B (originale)
>- Commit C
>- Commit Revert di B (nuovo)
>
>Potrebbe essere necessario nell'editor accettare i dati in arrivo (*incoming*) e fare il **push del revert del commit**.
>
>![[Pasted image 20230918180012.png]]
>
>Ora la repository è tornato allo stato del Commit A senza eliminare alcun commit dalla cronologia.

---
### GESTIONE DEI BRANCH
In Git, i **branch** sono una parte fondamentale della gestione del versionamento. Consentono di lavorare su diverse linee di sviluppo in modo isolato, facilitando la collaborazione e la gestione dei cambiamenti.

![[Pasted image 20230919082900.png|400]]

#### Creazione di un nuovo Branch
Per creare un nuovo branch in Git e spostarti su di esso, si utilizza il comando `git checkout -b` seguito dal nome del nuovo branch:
```bash 
git checkout -b <nome_del_branch>
```

Dove `<nome_del_branch>` è il nome del nuovo branch da creare.

Questo comando crea un nuovo branch basato sul branch corrente e sposta automaticamente il focus su di esso. Ora si può iniziare a lavorare sul nuovo branch senza influenzare il branch principale o altri branch.

#### Spostarsi in un Branch
Per modificare un branch esistente, è necessario prima spostarsi su di esso utilizzando `git checkout` seguito dal nome del branch:

```bash
git checkout <nome_del_branch>
```

Dove `<nome_del_branch>` è il nome del branch in cui spostarsi.

Ora si possono apportare le modifiche desiderate al codice, aggiungere nuovi commit e salvare le modifiche nel branch selezionato.

#### Unione di un Branch (merge)
Una volta completate le modifiche in un branch e si desideri integrare tali modifiche nel branch principale o in un altro branch, si può utilizzare il comando `git merge`, assicurandosi però di essere *nel branch di destinazione* (ad esempio, il branch principale) utilizzando `git checkout`.

Esegui il comando `git merge` seguito dal nome del branch da cui desideri unire le modifiche:

```bash
git merge <nome_del_branch>
```

Dove `<nome_del_branch>` è il nome del branch da unire al branch corrente.

Questo combinerà le modifiche apportate nel branch selezionato con il branch di destinazione. Se ci sono conflitti, bisogna risolverli manualmente prima di completare l'operazione di merge.

#### Eliminazione di un Branch
Per eliminare un branch dopo che le sue modifiche sono state integrate con successo, si può utilizzare il comando `git branch -d` seguito dal nome del branch:
```bash
git branch -d <nome_del_branch>
```

Se il branch contiene ancora modifiche che non sono state unite, dovrai utilizzare `-D` anziché `-d` per forzare l'eliminazione:
```bash
git branch -D <nome_del_branch>
```

Dove `<nome_del_branch>` è il nome del branch da eliminare.

Per i branch remoti si usa invece il comando `push`:
```bash
git push -d <nome_remoto> <nome_del_branch>
```

Nella maggior parte dei casi `<nome_remoto>` sarà `origin`.
#### Strategie per mantenere dei Branch
Le migliori strategie e organizzazioni per gestire i branch possono variare in base alle esigenze specifiche del progetto e del team, ma ci sono alcune linee guida generali che possono aiutare a mantenere un flusso di lavoro efficiente:
1. **Branch Principale Stabile (Main o Master)**: 
	- Mantieni un branch principale stabile (spesso chiamato "main" o "master") che rappresenta la versione di produzione (finale, stabile) del progetto;
	- Tutti i branch dovrebbero essere creati a partire da questo branch principale;
	- Nome suggerito: `main` o `master`.
2. **Feature Branches (Branch delle Funzionalità)**:
	- Crea un nuovo branch per ogni nuova funzionalità o attività specifica che richiede sviluppo;
	- Nome suggerito: `feature/nome-funzionalità`, ad esempio `feature/login`, `feature/carrello-acquisti`.
3. **Branch di Fix dei Bug**:
	- Crea un nuovo branch per ogni tentativo di correzione di bug;
	- Nome suggerito: `feature/nome-bug` o `feature/numero-bug`, ad esempio `fix/errore-login`, `fix/bug-547`.
4. **Branch di Sviluppo**:
	- Nel caso in cui il progetto abbia un ciclo di sviluppo complesso, i potrebbe utilizzare un branch di sviluppo separato da cui derivano i branch delle funzionalità;
	- I branch delle funzionalità vengono uniti prima a questo branch di sviluppo e poi, dopo i test, al branch principale;
	- Nome suggerito: `develop`.
5. **Pull Requests o Merge Requests**:
	- Utilizzare pull requests (in GitHub) per richiedere la revisione e l'approvazione delle modifiche prima di unire un branch;
	- Questo processo aiuta a garantire la qualità del codice e favorisce la collaborazione.
6. **Code Review**:
	- Promuovi la revisione del codice tra membri del team per identificare e risolvere problemi prima di unire i branch;
	- Fornisci feedback costruttivo e mantieni un registro delle discussioni sulla revisione del codice.
7. **Documentazione dei Branch**:
	- Tieni traccia di ogni branch, della sua finalità e del motivo per cui è stato creato;
	- Mantieni un registro delle decisioni prese per i branch e dei problemi aperti (magari in un `README.md`).
8. **Pulizia Periodica**:
	- Elimina i branch delle funzionalità una volta che sono stati uniti con successo;
	- Mantieni le repository pulito eliminando branch ormai obsoleti.

## SINCRONIZZAZIONE CON GITHUB
**GitHub** è una piattaforma di hosting di repository Git che consente la collaborazione e la condivisione di codice tra sviluppatori. Per collaborare con successo su GitHub, è importante comprendere come utilizzare `push` e `pull` per sincronizzare le repository locali con quelle remote su GitHub.

Di solito `origin` è il nome predefinito della repository remota.

> [!success] Consiglio di usare programmi per questo
### Pushing (Invio) delle modifiche
Il comando `git push` viene utilizzato per inviare le modifiche dalla tua repository locale a una repository remota su GitHub. Ecco come funziona:

```bash
git push origin <nome_del_branch>
```

Dove `<nome_del_branch>` è il nome del branch da mandare a GitHub.

Assicurati di essere nel branch giusto prima di eseguire il push.
### Pulling (Ricezione) le Modifiche
Il comando `git pull` viene utilizzato per ricevere le modifiche dalla repository remota su GitHub e aggiornare la tua repository locale. Ecco come funziona:

```bash
git pull origin <nome_del_branch>
```

Dove `<nome_del_branch>` è il nome del branch da ricevere da GitHub.

Questo comando combina in modo efficiente il `git fetch` per ottenere le modifiche dal repository remoto e il `git merge` per unire queste modifiche nel tuo branch locale.

## EGIT
> [!warning] Consiglio di usare GitHub Desktop invece di EGit

Dato che utilizziamo [Eclipse JEE](https://www.eclipse.org/downloads/packages/release/2022-12/r/eclipse-ide-enterprise-java-and-web-developers) come IDE, si può utilizzare Git (e quindi anche GitHub) attraverso una estensione dal marketplace.

> [!info] Normalmente, JEE ha già installato **EGit**
>Se non è installato:
>- Seleziona `Help -> Marketplace`;
>- Cercare `egit` ed installarlo.

### CONFIGURAZIONE EGIT
Ogni commit in EGit deve includere l'username e l'email dell'utente.
Questi attributi li si possono inserire andando in:
- `Window -> Preferences`
- `Version Control (Team) -> Git -> Configuration`
- `Add Entry...`

![[Pasted image 20230310130916.png|400]]
In questa schermata inserire come:
- Key: `user.name`
- Value: (nome utente usato su GitHub)

Ripetere questo procedimento inserendo una nuova entry con:
- Key: `user.email`
- Value: (email usata su GitHub)

Si possono scambiare le *viste* di eclipse in alto a destra. 
- Per utilizzare completamente git selezionare la vista GIT.
- Per ritornare a JEE selezionare la vista Java EE.
![[Pasted image 20230310131454.png]]

È utile inserire il Package Explorer nella vista GIT (`Window -> Show View -> Package Explorer`).

### CLONARE UNA REPOSITORY DA GITHUB

> [!warning]+ Creare un token di accesso personale
> Dal 2021 GitHub non supporta più l'autentificazione della password, quindi bisogna creare un *token di accesso personale*.
> - Accedere a GitHub, cliccare in alto a destra sul profilo e andare in `Settings`;
> - Sulla nuova pagina, sul lato sinistro cliccare `Developer Settings`;
> - Sulla nuova pagina, sul lato sinistro cliccare `Personal access tokens -> Tokens (classic)`;
> - `Generate new token -> Generate new token (classic)`
> - Dare un nome descrittivo al token in `Note` (es: Eclipse) e selezionare quando scadrà il token.
> - Selezionare le autorizzazioni del token (almeno `repo`) e `Generate token`
> - Copiare e salvare subito il token, perché non sarà più visibile!

Su eclipse con la vista GIT cliccare su `Clone a Git Repository`.

![[Pasted image 20230310133333.png]]

Dopo aver creato una nuova repository su GitHub inserire l'[[002 URL#^d0254a|URI]] usato per fare il clone (es: `https://github.com/UTENTE/ProgettoTest.git`) nel campo `URI`.

![[Pasted image 20230310133605.png|400]]

Inserire nel campo `User` l'username di GitHub e in `Password` il token di accesso personale.
Salvando nel Secure Store le credenziali, queste non verranno più richieste.
Passare avanti fino a quando non si deve inserire il path della repository locale. Inserire il path che vuoi.

Creare un progetto qualsiasi nel package explorer. Fare tasto destro sul progetto, `Team -> Share Project...`.
Nel campo repository ci dovrebbe essere la repository che abbiamo clonato. Selezionala e `Finish`.
Se ci sono già dei file sulla repository, fare tasto destro sul progetto `Team -> Pull` per ottenere tutti i branch e file esistenti.


### COMMIT SU EGIT
Inserire un file di prova (es: `Main.java`). Andare poi in basso (dove si trova la console) su `Git Staging`.

![[Pasted image 20230310134728.png|800]]

Trascinare tutti i file che si vogliono salvare nella repository (commit) da Unstaged Changes a Staged Changes, inserire un Messaggio di Commit, `Commit and Push`.

![[Pasted image 20230310134854.png|800]]

Su GitHub dovrebbero essere presenti tutti i file che abbiamo inserito.

![[Pasted image 20230310135109.png|800]]

Ogni volta che si modifica e si salva un file, si deve fare il commit. Questo commit avviene sia nella versione **Local** della repository (sul computer), sia nella versione **Remote** (su GitHub).

### BRANCHES SU EGIT
Diciamo che troviamo un problema su un file, ad esempio:
```Java
public class Main {
	public static void main(String[] args) {
		while(true) // Problema: loop infinito
			System.out.println("Chicken Soup");
	}
}
```

Invece di andare a modificare il file sul branch main, sarebbe molto più sicuro creare un nuovo branch utilizzato per il fix di questo problema.

Tasto destro sul branch main (local) nella vista `Git Repositories`, `Create Branch`.

![[Pasted image 20230310141007.png]]

![[Pasted image 20230310140731.png]]

Per metterlo anche sul remote, tasto destro sul nuovo branch, `Push Branch`.

Risolviamo il problema.
```Java
int i = 0, n = 3;
while(i < n) {
	System.out.println("Chicken Soup");
	i++;
}
```
Facciamo il commit.

Per applicare il fix alla versione live eseguiamo il merge, passare al branch main (local) da `Git Repositories`, tasto destro sul main, `Merge`, selezionare il branch che non sia il main. 
Fare il push del branch main (local) alla versione remote.

![[Pasted image 20230310141824.png]]

Per eliminare un branch da eclipse (in remote), tasto destro sul progetto, `Team -> Remote -> Push...`. 
Seleziona la repository, `Next`, nel campo `Remote ref to delete` selezionare il branch da eliminare, `Add Spec`, `Finish`.
Eliminare con il tasto destro il branch locale.

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
- [Git Docs](https://git-scm.com/docs)
- [Wikipedia1](https://it.wikipedia.org/wiki/Controllo_versione_distribuito), [Wikipedia2](https://it.wikipedia.org/wiki/Git_(software))
- [W3Schools](https://www.w3schools.com/git/default.asp?remote=github)
