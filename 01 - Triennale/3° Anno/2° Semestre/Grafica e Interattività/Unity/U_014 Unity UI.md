---
aliases:
  - Canvas
  - Rendering Mode del Canvas
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: 
cssclasses:
  - 
---
> [!warning]- Nuovi sistemi
>[UI Toolkit](https://docs.unity3d.com/Manual/UIElements.html) è il nuovo sistema per l'UI in Unity.

>**Unity UI** è un insieme di strumenti per lo sviluppo di [[Interfaccia GUI|interfacce utente]] per giochi e applicazioni. È un sistema di interfaccia utente *basato sui [[U_002 GameObject|GameObject]]* che utilizza i [[U_002 GameObject#Componenti dei GameObject|componenti]] e la vista di gioco per disporre, posizionare e modellare le interfacce utente. 

Non è possibile utilizzare Unity UI per creare o modificare le interfacce utente nell'Editor Unity. 

## Canvas
>Il **Canvas** è l'area all'interno della quale *devono trovarsi tutti gli elementi* dell'interfaccia utente. È un GameObject con un componente `Canvas` e tutti gli elementi dell'interfaccia utente devono essere figli di tale Canvas. Può essere creato con `GameObject ➡️ UI ➡️ Canvas`.

L'area del Canvas viene visualizzata come un rettangolo nella View.

![[Pasted image 20240518164010.png|800]]

Gli elementi dell'interfaccia utente nel Canvas vengono disegnati nello *stesso ordine in cui appaiono nella Hierarchy*. Il primo figlio viene disegnato per primo, il secondo figlio per secondo e così via. Se due elementi dell'interfaccia utente si sovrappongono, quello successivo apparirà sopra quello precedente.

## Rendering del Canvas
Il Canvas ha un'impostazione di **Rendering Mode** che può essere utilizzata per eseguire il rendering nello spazio dello schermo o nello spazio del mondo. Ci sono tre tipi di modalità.

![[Pasted image 20240518185053.png|400]]

### Screen Space - Overlay
Questa modalità di rendering posiziona gli elementi dell'interfaccia utente *sullo schermo renderizzato sopra la scena*. Se lo schermo viene ridimensionato o cambia risoluzione, il canvas cambierà automaticamente dimensioni per adattarsi a questo.

![[Pasted image 20240518184942.png]]

### Screen Space - Camera
È simile all'Overlay, ma in questa modalità di rendering il canvas è posizionato ad una determinata distanza *di fronte ad una specifica [[U_003 Telecamere in Unity|Camera]]*. Gli elementi dell'interfaccia utente sono renderizzati da questa telecamera, il che significa che le impostazioni della telecamera influenzano l'aspetto dell'interfaccia utente. 

### World Space
In questa modalità di rendering, il canvas si comporta *come qualsiasi altro oggetto della scena*. La dimensione della tela può essere impostata manualmente utilizzando la sua [[#Rect Transform|Trasformazione Rettangolare]] e gli elementi dell'interfaccia utente verranno renderizzati davanti o dietro gli altri oggetti della scena in base al posizionamento 3D. 

Questo è utile per le interfacce utente che devono essere *parte del mondo*.

![[Pasted image 20240518185412.png]]

## Rect Transform
Il **Rect Transform** è un nuovo componente di [[U_011 Traslare i GameObject#^c26b0d|trasformazione]] che viene utilizzato per tutti gli elementi dell'interfaccia utente al posto del normale componente Transform.

Le Rect Transform hanno posizione, rotazione e scala come le normali trasformazioni, ma hanno anche *una larghezza e un'altezza*, usate per specificare le dimensioni del rettangolo inerente all'oggetto dell'UI.

![[Pasted image 20240518190108.png]]

### Pivot
La posizione del **pivot** influisce sul *risultato* di una rotazione, di un ridimensionamento o di una scalatura. 

Quando il pulsante `pivot` della barra degli strumenti è impostato sulla modalità `pivot`, il pivot di una Trasformazione rettangolare può essere spostato nella View.

![[Pasted image 20240518190428.png]]

### Anchors

Gli **Anchors** sono visualizzati come quattro piccole maniglie triangolari nella View e le informazioni sugli ancoraggi sono visualizzate anche nell'Inspector.
Sono usate per bloccare un oggetto all'interno di un canvas, in modo che rimarranno allo stesso posto anche se il canvas viene modificato.

![[Immagine 2024-05-18 190826.gif]]

Nell'Inspector, attraverso il pulsante `Anchor Preset`, è possibile selezionare rapidamente alcune delle opzioni di ancoraggio più comuni.

![[Pasted image 20240518191144.png]]

## Oggetti dell'UI

### Text
L'oggetto **Text** (o *Label*) dispone di un'area di testo per l'immissione del testo che verrà visualizzato. È possibile impostare il tipo di carattere, lo stile del carattere, la dimensione del carattere e se il testo è dotato o meno di funzionalità di rich text.

![[Pasted image 20240518191439.png]]

È possibile modificare questo oggetto dallo script attraverso la classe `Text`.

### Image
Un'immagine può essere inserita nell'UI con l'oggetto **Image**.

![[Pasted image 20240518191816.png|400]]

### Sprites
Gli **Sprites** sono delle [[Textures|texture]] con alcune informazioni extra e sono utilizzate all'interno di UI e contesti 2D.

Le immagini possono essere *importate come sprite* selezionando `Sprite (2D/UI)` dalle impostazioni di `Texture Type`.

È possibile *applicare* uno sprite al componente Image nel campo `Source Image` e impostarne il colore nel campo `Colore`.



![[Pasted image 20240518192120.png|320]]

### Button

L'oggetto **Button** risponde a un clic dell'utente e viene utilizzato per *avviare o confermare un'azione*. Viene creato in `GameObject ➡️ UI ➡️ Legacy ➡️ Button`.

Esistono tre tipi di pulsanti
- *Interactable*: da attivare se si desidera che il pulsante accetti input.
- *Transition*: proprietà che determina il modo in cui il controllo risponde visivamente alle azioni dell'utente.
- *Navigation*: proprietà che determinano la sequenza dei controlli.

![[Pasted image 20240518192653.png]]

Il pulsante ha un singolo evento chiamato `On Click` che risponde quando l'utente fa clic.

Il testo del pulsante è figlio del componente del pulsante, che si trova nella hiearchy.

> [!example]- <font color="orange">Esempio</font>
>```CS
>using UnityEngine;
>using UnityEngine.UI;
>
>public class Example : MonoBehaviour 
>{
>	//Make sure to attach these Buttons in the Inspector
>	public Button buttonOne, buttonTwo, buttonThree;
>	
>	//Calls the TaskOnClick/TaskWithParameters/ButtonClicked method when you click the Button
>	void Start() 
>	{ 
>		buttonOne.onClick.AddListener(TaskOnClick);
>		
>		buttonTwo.onClick.AddListener(delegate {TaskWithParameters("Hello"); });
>		
>		buttonThree.onClick.AddListener(() => ButtonClicked(42));
>		buttonThree.onClick.AddListener(TaskOnClick); 
>	}
>	
>	//Output this to console when Button1 or Button3 is clicked 
>	void TaskOnClick() 
>	{ 
>		Debug.Log("You have clicked the button!"); 
>	}
>	
>	//Output this to console when the Button2 is clicked
>	void TaskWithParameters(string message) 
>	{ 
>		Debug.Log(message); 
>	}
>	
>	//Output this to console when the Button3 is clicked
>	void ButtonClicked(int buttonNo) 
>	{  
>		Debug.Log("Button clicked = " + buttonNo); 
>	} 
>}
>```

___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Manuale - Unity UI](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/index.html)
- [Manuale - Canvas](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/UICanvas.html)
- [Manuale - Rect](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/UIBasicLayout.html)
- [Manuale - Visual Components](https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/UIVisualComponents.html)