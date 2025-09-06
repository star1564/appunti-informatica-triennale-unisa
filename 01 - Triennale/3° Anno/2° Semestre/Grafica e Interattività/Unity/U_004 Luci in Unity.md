---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Unity
cssclasses:
  - 
---
## Fonti Luminose
Le fonti luminose sono trattate come [[U_002 GameObject|GameObject]].
### Point Lights
>Un **Point Light** si trova in un punto dello spazio e invia la luce *in tutte le direzioni* allo stesso modo. L'intensità *diminuisce con la distanza* dalla luce, raggiungendo lo zero a una determinata distanza. 

![[Pasted image 20240415194546.png]]

Le Point Light sono utili per simulare lampade e altre *fonti di luce locali* in una [[U_001 Unity#Scene|scena]].

### Spot Lights
>Come le Point Lights, anche le **Spot Lights** hanno una posizione e un intervallo specifico in cui la luce cade. Tuttavia, una luce spot *è vincolata a un angolo*, il che determina una regione di illuminazione a forma di cono. 

La luce diminuisce anche ai bordi del cono di un faretto. Allargando l'angolo, aumenta l'ampiezza del cono e, di conseguenza, le dimensioni di questa dissolvenza, nota come "penombra".

![[Pasted image 20240415194700.png]]

Le Spot Lights sono generalmente utilizzate per le *fonti di luce artificiale*, come torce, fari di automobili e fari di ricerca.

### Directional Lights
>Comportandosi per molti versi come il sole, le **Directional Lights** possono essere considerate come *sorgenti luminose distanti che esistono all'infinito*. Una luce direzionale non ha una posizione identificabile della sorgente e quindi l'oggetto luminoso può essere posizionato ovunque nella scena. 

Tutti gli oggetti della scena sono illuminati come se la luce provenisse sempre dalla stessa direzione. La distanza della luce dall'oggetto target non è definita e quindi *la luce non diminuisce*.

![[Pasted image 20240415195004.png]]

Le luci direzionali sono utili per creare effetti come la luce del sole nelle scene. 

## Light Modes

La proprietà **`Mode`** di una luce nell'Inspector (quando è selezionata una sorgente luminosa) può essere di tre tipi:
1. **Baked** - L'illuminazione diretta e indiretta di queste luci viene trasformata in una *lightmap*, un processo che può richiedere molto tempo. L'elaborazione di queste luci non ha costi di runtime, ma le ombre saranno fisse.
   
   ![[Pasted image 20240415192326.png]]
   
2. **Realtime** - L'illuminazione diretta e le ombre di queste luci sono in *tempo reale* e quindi non vengono inserite nelle lightmap. Il loro costo di runtime può essere elevato, ma le ombre saranno dinamiche.
3. **Mixed** - Si tratta di una modalità ibrida che offre un *mix* di funzioni in baked e in tempo reale, come l'illuminazione indiretta in baked e l'illuminazione diretta in tempo reale. Il comportamento di tutte le luci miste nella scena e il loro impatto sulle prestazioni dipendono dalla modalità di illuminazione della scena.

È importante notare che la modalità di una luce è rilevante solo se il sistema Baked Global Illumination è abilitato. Se non si utilizza alcun sistema di illuminazione globale o si utilizza solo il sistema di illuminazione globale in tempo reale di Enlighten, tutte le luci Baked e Mixed si comporteranno come se la loro proprietà Mode fosse impostata su Realtime.

> [!note]- Quando utilizzare le Modalità
>![[Immagine 2024-04-15 193743.png]]

___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [Manuale - Lighting Sources](https://docs.unity3d.com/Manual/Lighting.html)
- [Manuale - Light Modes](https://docs.unity3d.com/Manual/LightModes.html)