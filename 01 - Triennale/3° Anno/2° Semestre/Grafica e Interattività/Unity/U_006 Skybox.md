---
aliases: 
tags:
  - corsi/informatica/grafica_interattivita
paragrafo: Unity
cssclasses:
  - 
---
>La **Skybox** è un cubo con una [[Textures|texture]] diversa *su ogni faccia*. 

![[Pasted image 20240416143420.png|500]]

Essenzialmente, quando si utilizza uno skybox per eseguire il rendering di un cielo, Unity posiziona la Scena all'interno del cubo skybox. Unity esegue il rendering dello skybox per primo, quindi il cielo *viene sempre visualizzato sul retro*.

### Skybox Procedurale
Quando viene creata una nuova Scena su Unity, la skybox è impostata ad una **Procedurale**, dove viene calcolata anche la posizione della *[[U_004 Luci in Unity#Directional Lights|Luce Direzionale]] Principale* (il sole).

![[Pasted image 20240416143627.png]]

## Creare ed Applicare la Skybox

> [!tip]- Importare la skybox
> - Importare il file della texture da applicare
>- Modificare il tipo del file, da `2D` a `Cube`, ed applicare i cambiamenti
>
>![[Pasted image 20240416144125.png]]

### Creazione
Per creare la skybox è necessario: 
- Creare un nuovo materiale con `(Tasto destro su uno spazio vuoto del project view) ➡️ Create ➡️ Material`
- Modificare il tipo di [[Shaders|shader]] del materiale da `Standard` a `Skybox/Cubemap`
- Trascinare la texture modificata all'interno del quadrato del materiale

![[Pasted image 20240416145233.png]]

> [!tip]+ Creare una Skybox da 6 file della Cubemap
>- Creare un nuovo materiale con `(Tasto destro su uno spazio vuoto del project view) ➡️ Create ➡️ Material`
>- Modificare il tipo di shader del materiale da `Standard` a `Skybox/6 Sided`
>- Inserire tutti i file nei rispettivi quadrati
>
>![[Pasted image 20240604115025.png]]
>
> Inoltre, per rimuovere le linee bianche tra le facce dei quadrati, è possibile modificare il warp mode di tutte le texture delle facce in `Clamp`.
> 
> ![[Pasted image 20240605133538.png]]

### Applicazione
Per applicare la skybox, è possibile *trascinarla sulla scena*, oppure si può applicare dal menu Lighting:
- Aprire il menu del Lighting da `Window (menu in alto) ➡️ Rendering ➡️ Lighting`, è consigliato spostare il tab di lighting vicino all'inspector

![[Pasted image 20240416144804.png|300]]
- Passare ad `Environment`
- Trascinare il materiale con la texture all'interno di `Skybox material` o selezionarla dal menu

![[Pasted image 20240416145405.png]]



   


___
[[000 Indice Grafica|↖ Ritorna all'indice ↖]]

Altri collegamenti: 
- [The RealTime Essentials - Video](https://www.youtube.com/watch?v=zdJhDRsMVGE&list=PLiwlO4qBCuwQ2zKQEtkBIZ0lr-FOKUp5U&index=4)