---
aliases: 
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Interfacce Grafiche in Java
cssclasses:
  - 
---
Esistono diverse *componenti interattive* in Java.
Le azioni che vengono eseguite alla interazione sono specificate da un [[028 Action Listener|Action Listener]].

Molti di questi componenti utilizzano il metodo **`setBounds(x1, y1, width, height)`** per gestire le dimensioni.
Viene usato spesso **`setLayout(null)`** per posizionare manualmente i componenti utilizzando le coordinate assolute (invece di posizionarle in base al layout).

## ETICHETTA (JLabel)
L'oggetto di una classe **`JLabel`** è un componente per posizionare del testo in un contenitore. Viene usato per mostrare una singola linea di una frase.
Il testo presente può essere cambiato nel codice ma non dall'utente.

### Costruttore:
```Java
JLabel();             // Crea una etichetta vuota
JLabel(String s);     // Crea una etichetta inizializzata con la stringa s
```

### Metodi:
```Java
getText();                             // Ritorna il testo sotto forma di stringa contenuto dall'etichetta
setText(String text);                  // Imposta il testo da mostrare
setHorizontalAlignment(int alignment); // Imposta l'allineamento del testo ORIZZONTALE (JLabel.CENTER, LEFT, ecc.)
setVerticalAlignment(int alignment);   // Imposta l'allineamento del testo VERTICALE (JLabel.TOP, BOTTOM, ecc)
setForeground(Color c);                // Imposta il colore del font
setFont(new Font("Font", Style, S));   // Imposta il font ("Nome Font", Stile Font (Font.PLAIN, BOLD, ITALICS), int size)
setBorder(BorderFactory.createLineBorder(Color c, dim));  // Imposta un bordo attorno all'etichetta
```

> [!example]- <font color="orange">Esempio Utilizzo</font>
>```Java
>import javax.swing.JFrame;
>import javax.swing.JLabel;
>
>public class EtichettaDemo extends JFrame{
>	
>	JLabel label;
>	EtichettaDemo() {
>		this.setSize(300, 300);
>		this.setLayout(null);
>		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		this.setVisible(true);
>		
>		label = new JLabel();
>		label.setText("Testo Label");
>		label.setBounds(50, 100, 180, 50);
>		
>		this.add(label);
>	}
>	
>	public static void main(String[] args) {
>		EtichettaDemo e = new EtichettaDemo();
>	}
>}
>```
>![[Pasted image 20221208203627.png]]

## PULSANTE (JButton)
La classe **`JButton`** serve per definire dei pulsanti con un'etichetta. Quando viene premuto, produce un'Azione.

### Costruttore:
```Java
JButton();             // Crea un bottone vuoto
JButton(String s);     // Crea un bottone inizializzato con la stringa s
```

### Metodi:
```Java
setText(String text);    // Imposta il testo da mostrare
setEnabled(boolean b);   // È usato per abilitare o disabilitare un pulsante
setFocusable(boolean b); // È usato per abilitare o disabilitare l'effetto di focus attorno al nome del pulsante
setToolTipText(String descrizione); // Imposta una descrizione da far apparire quando si passa il mouse sul pulsante
```

> [!example]- <font color="orange">Esempio Utilizzo</font>
>```Java
>import javax.swing.JButton;
>import javax.swing.JFrame;
>
>public class BottoneDemo extends JFrame{
>	
>	JButton button;
>	BottoneDemo() {
>		this.setSize(300, 300);
>		this.setLayout(null);
>		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		this.setVisible(true);
>		
>		button = new JButton("Testo button");
>		button.setBounds(50, 100, 180, 50);
>		button.setFocusable(false);
>		
>		this.add(button);
>	}
>	
>	public static void main(String[] args) {
>		BottoneDemo b = new BottoneDemo();
>	}
>}
>
>```
>![[Pasted image 20221208204044.png]]

## PANNELLO (JPanel)
La classe **`JPanel`** è un componente che contiene altri componenti. Un pannello ha un proprio [[030 Layout JFrame|Layout Manager]] (quello predefinito è il Flow Layout).

### Costruttore:
```Java
JPanel(); // Crea un bottone vuoto
```

### Metodi:
```Java
setBackground(Color c);      // Imposta il colore dello sfondo del pannello
setLayout(LayoutManager lm); // Imposta il Layout Manager che usa il pannello
add(Component c);            // Si possono aggiungere componenti al pannello
```

> [!example]- <font color="orange">Esempio</font>
>```Java
>import java.awt.Color;
>import javax.swing.JFrame;
>import javax.swing.JLabel;
>import javax.swing.JPanel;
>
>public class Main {
>
>	public static void main(String[] args) {
>		
>		JLabel label = new JLabel("Hi");
>		
>		JPanel redPanel = new JPanel();
>		redPanel.setBackground(Color.red);
>		redPanel.setBounds(0, 0, 250, 250);
>		
>		JPanel bluePanel = new JPanel();
>		bluePanel.setBackground(Color.blue);
>		bluePanel.setBounds(250, 0, 250, 250);
>		
>		JPanel greenPanel = new JPanel();
>		greenPanel.setBackground(Color.green);
>		greenPanel.setBounds(0, 250, 500, 250);
>		greenPanel.setLayout(null);
>		
>		JFrame frame = new JFrame();
>		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
>		frame.setSize(750, 750);
>		frame.setVisible(true);
>		frame.setLayout(null);
>		
>		frame.add(redPanel);
>		frame.add(bluePanel);
>		frame.add(greenPanel);
>		
>		greenPanel.add(label);
>		label.setBounds(100, 0, 100, 100); // Le coordinate sono relative a greenPanel
>	}
>}
>```
>![[Pasted image 20221209151413.png|520]]

## CAMPO DI TESTO (JTextField)
La classe **`JTextField`** permette la modifica sia da parte dell'utente che da parte del programmatore.

![[Pasted image 20221209180247.png]]

```Java
JTextField tf = new TextField();
tf.setPreferredSize(new Dimention(250, 40));  // Settaggio della dimensione dell'intero textfield preferita
text.setColumns(7); // Imposta la dimensione predefinita di spazi vuoti
frame.add(tf);
```

Sarebbe utile combinarlo con altri componenti per ricevere il testo scritto dall'utente. 
È possibile cambiare il colore, la dimensione e la famiglia del font.

## CASELLA DI CONTROLLO (JCheckBox)
La classe **`JCheckBox`** è un componente che può essere selezionato o deselezionato.

![[Pasted image 20221209181152.png]]

```Java
JCheckBox cb = new JCheckBox();
cb.setText("Testo 1");
cb.setFocusable(false);

JCheckBox cb2 = new JCheckBox();
cb2.setText("Testo 2");
cb2.setFocusable(false);

frame.add(cb);
frame.add(cb2);
```

Si può controllare lo stato della casella con
```Java
cb.isSelected();  // Ritorna true se selezionata, false altrimenti
```

È possibile cambiare il colore, la dimensione e la famiglia del font del testo.

## BOTTONI DI SELEZIONE (JRadioButton)
La classe **`JCheckBox`** è un componente che crea diversi pulsanti "radio". Questo significa che in questo gruppo di pulsanti *solo uno può essere selezionato*.

![[Pasted image 20221209183938.png]]

```Java
JRadioButton pizzaButton = new JRadioButton("Pizza");
JRadioButton hamburgerButton = new JRadioButton("Hamburgher");
JRadioButton hotdogButton = new JRadioButton("Hotdog");
```

Una volta creati i bottoni, bisogna creare anche il gruppo che li contiene. Poi si aggiungono al frame i pulsanti.
```Java
ButtonGroup group = new ButtonGroup();		

group.add(pizzaButton);
group.add(hamburgerButton);
group.add(hotdogButton);

frame.add(pizzaButton);
frame.add(hamburgerButton);
frame.add(hotdogButton);
```

Si può controllare lo stato della casella con un semplice action listener.

È possibile cambiare il colore, la dimensione e la famiglia del font del testo.

## SELEZIONE MULTIPLA (JComboBox)
La classe **`JComboBox`** è un componente che *combina* diversi bottoni o campi di testo editabili in una lista a cascata.

![[Pasted image 20221209185228.png]]

Per utilizzare la combo box bisogna passare al costruttore un array di oggetti (ad esempio delle stringhe).

```Java
String[] animali = {"cane", "gatto", "uccello"};

JComboBox comboBox = new JComboBox(animali);

frame.add(comboBox);
```

Una volta creato la combo box, verifichiamo tramite un action listener e dei metodi particolari quale elemento è stato selezionato.
```Java
comboBox.addActionListener(e -> {
	System.out.println("Indice " + comboBox.getSelectedIndex() + ": " + comboBox.getSelectedItem());
});
// Stampa su console l'indice dell'oggetto e l'oggetto stesso.
```

È possibile rendere editabile la combo box con `.setEditable(true)`.

Si può sapere il numero attuale di oggetti nella combo box con `.getItemCount()`.
Si può aggiungere un oggetto con `.addItem(Object)` e rimuovere un oggetto con `.removeItem(Object)`. Queste hanno le varianti con gli indici.

## SCROLL (JScrollPane)
La classe **`JScrollPane`** è un componente che si inserisce a destra di una `JTextArea`.
Si aggiunge *solo lo scroll* alla finestra.

```Java
JTextArea ta = new JTextArea(10, 15);    
// TextArea con 10 righe (se ne aggiungono automaticamente altre se necessario) e 15 colonne (restano lo stesso numero)

JScrollPane sp = new JScrollPane(ta, JScrollPane.VERTICAL_SCROLLBAR_ALWAYS, JScrollPane.HORIZONTAL_SCROLLBAR_NEVER);
// Si aggiunge lo scroll alla TextArea

frame.add(sp); // SI AGGIUNGE SOLO LO SCROLL ALLA FINESTRA
```

## BARRA MENÙ (JMenuBar, JMenu, JMenuItem)
Ci sono essenzialmente tre classi da usare:
- **`JMenuBar`**: è un componente che inserisce al di sotto del titolo della finestra una barra dove verranno inseriti tutti i menù;
- **`JMenu`**: è un menù che può contenere degli `JMenuItem`;
- **`JMenuItem`**: è l'elemento gerarchicamente più piccolo del menù.

![[Pasted image 20230207124901.png]]

```Java
JMenuBar mb = new JMenuBar();
frame.setJMenuBar(mb);

JMenuItem cut = new JMenuItem("cut");
JMenuItem copy = new JMenuItem("copy");
JMenuItem paste = new JMenuItem("paste");
JMenuItem selectAll = new JMenuItem("selectAll");

JMenu file = new JMenu("File");
JMenu edit = new JMenu("Edit");
JMenu help = new JMenu("Help");

edit.add(cut);
edit.add(copy);
edit.add(paste);
edit.add(selectAll);

mb.add(file);
mb.add(edit);
mb.add(help);
```

Si aggiunge un [[028 Action Listener|Action Listener]] normalmente.
```Java
// Funzionano solo per i JMenuItem
copy.addActionListener(e -> {
	copia();
})
```

## BROWSER FILE (JFileChooser)
Il **`JFileChooser`** è una finestra già creata che si apre a comando per selezionare un file dal computer.

```Java
JFileChooser fc = new JFileChooser();
int returnVal = fc.showOpenDialog(componenteGenitore); // Il componente genitore è quello che ha fatto aprire il file chooser
if(returnVal == JFileChooser.APPROVE_OPTION) { 
	// Se il file è stato scelto, si entra qui
	System.out.println("Hai scelto di aprire questo file: " + fc.getSelectedFile().getName());
}
```

![[Pasted image 20230207130701.png]]

## ALTRI COMPONENTI
- Slider;
- Barra Progresso;
- Toolbar;
- Tabs;
- Password Field;
- ...

### BORDO TITOLATO
Si può aggiungere un bordo ad un pannello per rendere più leggibile un gruppo (per esempio un gruppo di radio button).

![[Pasted image 20221209194313.png]]

```Java
panel.setBorder(new TitledBorder(new EtchedBorder(), "Scegli Uno"));
panel.add(pizzaButton);
panel.add(hamburgerButton);
panel.add(hotdogButton);
```

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
- [[Interfaccia GUI|Interfaccia GUI (SO)]]
- [Tutti i Componenti su JavaPoint](https://www.javatpoint.com/java-swing)
- [Bro Code (Video)](https://youtu.be/dyDDUndlMnU?list=PLZPZq0r_RZOMhCAyywfnYLlrjiVOkdAI1)