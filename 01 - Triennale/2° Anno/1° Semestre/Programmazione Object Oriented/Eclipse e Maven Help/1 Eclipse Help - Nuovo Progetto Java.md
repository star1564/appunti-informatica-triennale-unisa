---
tags:
  - corsi/informatica/programmazione_object_oriented
paragrafo: Eclipse Help
---
Per poter programmare in Java si deve scaricare la JDK (ultima versione) dal [sito ufficiale di Oracle](https://www.oracle.com/java/technologies/javase/jdk15-archive-downloads.html).

In questo corso è consigliato (per Java) usare come ambiente di programmazione (IDE) [Eclipse](https://www.eclipse.org/downloads/).

Per creare un nuovo (**semplice**) progetto java seguire queste istruzioni:
1. Quando non ci sono progetti, creane uno dal menù a sinistra, ![[Pasted image 20220922105704.png]] oppure attraverso click destro del mouse > New > Java Project; ![[Pasted image 20220922110028.png|430]]
2. Scegliere il <font color="CC241D">nome del progetto</font>, il <font color="61AFEF">compilatore</font> (dipende dalla JDK installata), controllare che il <font color="lime">module-info è disattivato</font>, premere Next per altre opzioni oppure <font color="yellow">Finish</font>; ![[Pasted image 20220922110719.png|600]]
3. Per creare una [[001 Introduzione a OOP#CLASSE|Classe]] si deve fare click destro sul progetto > New > Class; ![[Pasted image 20220922111644.png|500]]  ^da7592
4. Inserire il <font color="CC241D">nome della classe</font>, se si vuole creare un metodo <font color="61AFEF">main</font> spuntare la casella, premere <font color="lime">Finish</font>; ![[Pasted image 20220922112123.png]]
5. Inserire questo codice di stampa per provare il programma ed eseguire il codice con il pulsante in alto a sinistra;
```java
public class Main {
	public static void main(String[] args) {
		System.out.println("Hello Java"); //Stampa la frase
	}
}
```
![[Pasted image 20220922112951.png]]
![[Pasted image 20220922113137.png]]

> [!error]- Errore riguardo la "Dichiarazione di un pacchetto"
> Se durante (o prima) della compilazione si ha il seguente errore:
> >Must declare a named package because this compilation unit is associated to the named module 'x'
>
> Eliminare il file `module-info.java`.
> ![[Pasted image 20220922113859.png]]

%%[[000 Indice POO]]%%