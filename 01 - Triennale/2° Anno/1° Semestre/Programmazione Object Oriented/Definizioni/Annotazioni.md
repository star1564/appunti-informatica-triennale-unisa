---
aliases: Javadoc
cssclasses:
tags:
  - corsi/informatica/programmazione_object_oriented
---


Una **Annotazione** in Java è un'etichetta che rappresenta un [[Metadato]] usato per indicare delle informazioni aggiuntive che possono essere usate dal compilatore e dalla JVM.
Si indicano inserendo prima della keyword una **`@`**.

## ANNOTAZIONI FUNZIONALI
### OVERRIDE
L'annotazione **`@Override`** assicura che sia usato il metodo della [[015 Ereditarietà#^28eda7|sottoclasse]] rispetto a quello della superclasse.

> [!example]- <font color="orange">Esempio</font>
>```Java
>class Animal{
>	void eatSomething(){
>		System.out.println("eating something");
>	}
>}
>
>class Dog extends Animal{
>	@Override
>	void eatsomething(){
>		System.out.println("eating foods");
>	}
>}
>
>class TestAnnotation1{
>	public static void main(String args[]){
>		Animal a=new Dog();
>		a.eatSomething(); //should be eatSomething
>	}
>}
>```

### SUPPRESS WARNINGS
L'annotazione **`@SuppressWarnings`** è usata per sopprimere i warnings creati dal compilatore.

## ANNOTAZIONI DESCRITTIVE (JAVADOC)
Inserendo sopra il codice di una [[Classi|Classe]], [[Metodi|Metodo]] o [[011 Variabile Esemplare|Variabile di Stato]] i seguenti caratteri `/**` e premendo `Invio`, verranno *automaticamente* dedotte le annotazioni necessarie per descrivere qualcosa.

Se non c'è una annotazione all'interno di un commento del genere, il testo verrà utilizzato come descrizione.

Passando con il cursore sopra quello che è stato descritto si otterranno le informazioni.

> [!example]- <font color="orange">Esempio</font>
>```Java
>/**
> * Descrizione della classe
> * @author Saber
> */
>class Test1{
>	/**
>	 * Descrizione variabile interna
>	 */
>	private int vari;
>	
>	/**
>	 * Descrizione del metodo
>	 * @param a primo elemento
>	 * @param b secondo elemento
>	 * @return la somma di a+b
>	 */
>	public int add(int a, int b) {
>		return a+b;
>	}
>	
>	/**
>	 * Descrizione di un metodo void
>	 */
>	public void altro(){...}
>
>	public int getVari(){...}
>	public void setVari(int vari){...}
>
>}
>
>
>public class Main {
>
>	public static void main(String[] args) {
>		Test1 t = new Test1();
>		int altro = t.add(6, 5);
>		System.out.println(altro);
>	}
>
>}
>```
>![[Pasted image 20221124110555.png]]

### PARAM
L'annotazione **`@param [parametro]`** è usata per dare una descrizione ad un parametro di un metodo.

### RETURN 
L'annotazione **`@return`** è usata per dare una descrizione del valore di ritorno di un metodo.

### AUTHOR
L'annotazione **`@author`** è usata per segnalare l'autore del codice in questione (utile per progetti di gruppo).



%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%
