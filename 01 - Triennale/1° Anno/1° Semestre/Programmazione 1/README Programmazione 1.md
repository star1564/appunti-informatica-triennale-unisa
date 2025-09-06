> [!warning] Non ho scritto note sul corso di Programmazione 1 perché non conoscevo ancora Obsidian

Utilizza come linguaggio di programmazione [C](https://it.wikipedia.org/wiki/C_(linguaggio_di_programmazione)).

Ho un paio di cose da dire a chi si addentra in questo corso.

## Studia dal libro!
Per conoscere le basi della programmazione (e anche C) **consiglio altamente** di leggere i capitoli introduttivi del libro.

| Titolo              | Autore      | Editore | Anno | ISBN          |
| ------------------- | ----------- | ------- | ---- | ------------- |
| Programmazione in C | Kim N. King | Apogeo  | 2009 | 9788838785825 |


Consiglierei più la versione inglese, ma anche quella in italiano va bene.

> [!tip] Suggerimento
> Non dire mai "vettore" ma "**ARRAY**". Sono due cose diverse. È una delle pecche della traduzione italiana.

Non posso descrivere quanto questo libro mi abbia aiutato nel mio percorso introduttivo all'informatica e alla programmazione. Infatti, lo raccomando *soprattutto a chi non ha mai programmato*.


## Segui le lezioni!
Lo do per scontato, ma **è molto importante seguire anche le lezioni**. Utilizza il libro affianco alle lezioni per un miglior apprendimento.

## Dove posso scrivere il codice?
In questo corso è **consigliato dai docenti** di utilizzare *semplici editor di testo*. 
- Il blocco note (notepad) predefinito del tuo sistema operativo
- [Notepad++](https://notepad-plus-plus.org/) (*consigliato*)

> [!error] Non utilizzare gli IDE per QUESTO CORSO!
> I professori consigliano questo sempre per migliorare l'apprendimento iniziale.
> 
> Un IDE (Integrated Development Environment) è un software che raggruppa gli strumenti di sviluppo più utilizzati, come:
> - editor
> - compilatore
> - debugger
> - suggerimenti
> - AI
>   
>![[Pasted image 20250327133117.png]]
>   
> Per i futuri corsi, invece, è altamente consigliato (se non obbligatorio) utilizzarli. MA NON PER QUESTO CORSO.


### Evidenziamento del Codice
All'inizio potrebbe essere consigliato anche (assolutamente non obbligatorio) di scrivere codice "in bianco e nero", ovvero *senza evidenziamento o colorazione delle parole*. Questo è utile per le prime ore di apprendimento della programmazione per comprendere i concetti fondamentali senza troppe distrazioni.

![[Pasted image 20250327124937.png|200]]

Ma, dopo un poco, questo potrebbe diventare stancante, soprattutto quando la dimensione di codice in un file aumenta. Qui viene in soccorso *l'evidenziamento e la colorazione delle parole*. Ad ogni tipo di parola viene assegnato un colore specifico.

![[Pasted image 20250327125446.png|200]]


Potresti, ad esempio, scambiare il blocco note per Notepad++ a circa 1/4 del corso, oppure quando si ha già capito come funziona la programmazione.

## Come posso eseguire il codice?

Il linguaggio C è utilizzato nei seguenti corsi:
- Programmazione 1 (Anno 1, Semestre 1)
- Programmazione e Strutture Dati (Anno 1, Semestre 2)
- Sistemi Operativi (Anno 2, Semestre 1)

Per poter compilare (creare un eseguibile) è necessario il programma da linea di comando [gcc](https://gcc.gnu.org/). Questo *non è nativamente supportato da Windows*. 

> [!error] NON consiglierei di installare Cygwin (su Windows)

Invece, consiglio di iniziare a imparare le basi di Linux. Questo è un sistema operativo che ti accompagnerà per tutto il tuo percorso universitario *E LAVORATIVO* (è davvero così importante).

> [!success] Ho una sezione su Linux che spiega le basi generali [[002 Linux|qui]]


### Quale distribuzione scegliere e dove installarle
Si può installare qualsiasi distribuzione per questo corso (e quelli seguenti), ma raccomando uno di questi
- *Basate su Debian*
	- [Ubuntu](https://www.ubuntu-it.org/)
	- [Debian](https://www.debian.org/)
	- [Linux Mint](https://www.linuxmint.com/)
- *Basate su Red Hat*
	- [Fedora](https://fedoraproject.org/it/)

Ci sono tre modi per installare Linux:
1. *Su una macchina reale*
	- Come sistema operativo principale (magari su un portatile da portare all'università, Linux è abbastanza leggero e non richiede hardware nuovo o potente, quindi vanno bene anche portatili relativamente vecchi o usati)
	- Con il Dual Boot con Windows 
		- Ubuntu (ed alcune altre distribuzioni) permette durante l'installazione di installarsi accanto a Windows in modo semplice e diretto
			- Se si vuole disinstallare il dual boot, cerca delle guide prima di fare di testa tua!
		- Altre distribuzioni si possono installare "manualmente" sempre durante l'installazione accanto a Windows, ma raccomando di guardare qualche tutorial
2. *Su una macchina virtuale* (come [VirtualBox](https://www.virtualbox.org/))
	- Un'opzione abbastanza fattibile, ma è lento (molto lento in alcuni casi)
3. *Terminale Linux (Ubuntu)* su [Windows Store](https://apps.microsoft.com/detail/9pdxgncfsczv) 
	- **NON LO RACCOMANDO**, alcuni comandi necessari per il corso di [[000 Indice SO|Sistemi Operativi]] non funzionano bene

Puoi installare gnu su linux con uno dei seguenti comandi:
- Basati su Debian: `sudo apt install gcc`
- Basati su Fedora: `sudo dnf install gcc`
