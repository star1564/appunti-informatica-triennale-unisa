---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Unity
cssclasses:
  - 
---
>I **Materiali** sono definizioni di *come* deve essere [[005 Radianza#Rendering|renderizzata]] una superficie.
>
>Un Materiale può *contenere riferimenti* ad una [[Textures|texture]], in modo che lo [[Shaders|shader]] del Materiale possa utilizzare le texture durante il calcolo del colore della superficie di un oggetto.

> [!tip] Creazione
>  `(Tasto destro su uno spazio vuoto del Project View) ➡️ Create ➡️ Material`

Per impostazione predefinita, ai nuovi materiali viene assegnato lo Standard Shader, con tutte le proprietà della mappa vuote.

Una volta creato il Materiale, è possibile applicarlo a un oggetto e modificarne tutte le proprietà nell'*Inspector*. Per applicarlo a un oggetto, basta *trascinarlo dalla Project View* a qualsiasi oggetto della Scena o della Gerarchia.

![[Pasted image 20240416090432.png|300]]

> [!tip]- Materiale invisibile
> Per rendere un GameObject invisibile, disabilitare il Mesh Renderer
> ![[Pasted image 20240603151329.png]]

## Proprietà
Le proprietà fondamentali nell'Inspector sono:
- **Rendering Mode** - Consente di scegliere se l'oggetto utilizza la *trasparenza* e, in tal caso, quale modalità di fusione utilizzare.

- **Albedo** - Controlla il *colore di base* del materiale. Può anche essere impostato ad una *texture*, in questo modo il materiale utilizzerà la texture. Si può modificare la *trasparenza* del materiale (se è abilitata dalla rendering mode) attraverso il valore `A` dell'albedo.

![[Pasted image 20240416094906.png|300]]

> [!tip]- Come impostare una texture
> Trascinare il file della texture sul quadrato vicino all'Albedo oppure selezionare il file dal cerchio vicino al quadrato.
> 
> ![[Pasted image 20240416104043.png]]
> 
> Nel menù del materiale è possibile anche modificare il **Tiling** che permette di coprire un'ampia superficie *senza rendere visibili* i collegamenti.
> 
> ![[Pasted image 20240416104658.png]]


- **Metallicità (Standard Shader)** - Determina il grado di "*metallicità*" della superficie. Quando una superficie è più metallica, [[005 Radianza#Riflessione|riflette]] maggiormente l'ambiente e il suo colore albedo diventa meno visibile.

![[Pasted image 20240416094039.png|800]]

> [!note]- Specularità (Standard Shader - Specular Setup)
> Gli effetti [[006 Illuminazione Locale#Speculare|speculari]] (si trova all'interno di Standard Shader - Specular Setup) sono essenzialmente i riflessi diretti delle sorgenti di luce nella Scena, che in genere appaiono come riflessi luminosi e brillano sulla superficie degli oggetti (anche se i riflessi speculari possono essere sottili o diffusi). 
> 
> Sia l'impostazione Speculare che quella Metallica *producono riflessi speculari*, quindi la scelta di quale utilizzare è più che altro una questione di impostazione e di preferenze artistiche.
> 
> ![[Pasted image 20240416100147.png|800]]

- **Smoothness** - Una superficie liscia ha un dettaglio della microsuperficie molto basso, o addirittura nullo, quindi la luce rimbalza in modo *uniforme*, creando riflessi chiari. Una superficie ruvida presenta picchi e avvallamenti elevati nei dettagli della microsuperficie, per cui la luce rimbalza in un'*ampia gamma di angoli* che, se mediati, creano un colore diffuso senza riflessi chiari.

![[Pasted image 20240416094258.png|800]]

![[Pasted image 20240416095815.png|800]]

- **Normal Map** - Sono un tipo di *Bump Map*, un tipo speciale di texture che consente di aggiungere a un modello dettagli di superficie come *protuberanze, solchi e graffi* che catturano la luce come se fossero rappresentati da una geometria reale.

![[Pasted image 20240416100704.png|800]]

> [!tip]- Come impostare una Normal Map
> Trascinare il file della Normal Map (è un immagine di colore blu) sul quadrato vicino a Normal Map oppure selezionare il file dal cerchio vicino al quadrato.
> 
> Potrebbe dire che il file non è corretto, ma è perché il file non è impostato a `Normal Map`, si può sistemare facilmente con il pulsante apposito o andando a cambiare il tipo all'interno del file.
> 
> Si può anche modificare il valore dell'intensità della Normal Map.
> 
> ![[Pasted image 20240416104311.png]]

- **Height Map** - La Height Map *sposta effettivamente le aree della texture superficiale visibile*, per ottenere una sorta di effetto di occlusione a livello di superficie.

![[Pasted image 20240416100630.png|500]]

> [!tip]- Come impostare una Height Map
> Trascinare il file della Height Map (è un immagine di colore bianco e nero) sul quadrato vicino a Height Map oppure selezionare il file dal cerchio vicino al quadrato.
>
>Si può anche modificare il valore dell'intensità della Height Map.
> 
> ![[Pasted image 20240416104502.png]]


- **Occlusion Map** - La Occlusion Map viene utilizzata per fornire informazioni su quali aree del modello devono ricevere un'illuminazione indiretta alta o bassa. Sono prodotte da 3D artists.

![[Pasted image 20240416101134.png|300]]

- **Emission** - L'aggiunta dell'emissione a un Materiale lo fa *apparire come una fonte di luce* visibile nella Scena. Le proprietà di emissione del materiale controllano il colore e l'intensità della luce emessa dalla superficie di un materiale.

![[Pasted image 20240416102742.png|500]]

## Tiling delle Texture

Quando si applica una texture (o una normal map) ad un oggetto grande, questa potrebbe risultare allargata.

![[Pasted image 20240604110953.png|500]]

Per risolvere questo problema è necessario modificare il **Tiling** del materiale.

![[Pasted image 20240604111105.png|400]]


Di solito la proporzione ideale tra `x` e `y` è *1:1*, ma questa può dipendere dalla grandezza della texture e dell'oggetto.


___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Manuale - Texture](https://docs.unity3d.com/Manual/Textures.html)
- [Manuale - Standard Shader Material Inspector reference](https://docs.unity3d.com/Manual/StandardShaderMaterialParameters.html)
- [Applicare Materiali, Textures e Maps - Video](https://www.youtube.com/watch?v=aiTl7B2xTmA&list=PLiwlO4qBCuwQ2zKQEtkBIZ0lr-FOKUp5U&index=2)