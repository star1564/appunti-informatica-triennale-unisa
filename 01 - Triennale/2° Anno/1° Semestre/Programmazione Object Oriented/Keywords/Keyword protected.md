---
aliases:
  - protected
tags:
  - corsi/informatica/programmazione_object_oriented
---
![[Tipi, Signatures e Visibilità (IS)#^3e550d]]

All'interno della [[015 Ereditarietà|Derivazione di una classe]], una sottoclasse può modificare gli [[011 Variabile Esemplare|attributi]] della sua superclasse attraverso la keyword **`protected`**.

La sottoclasse può anche accedere direttamente ai campi attributi senza metodo get.

Per esempio, la sottoclasse `Student` accede ai campi di `Person` in questo modo:
```Java
class Person {
  protected String fname = "John";
  protected String lname = "Doe";
  protected String email = "john@doe.com";
  protected int age = 24;
}

class Student extends Person {
  private int graduationYear = 2018;
  
  public static void main(String[] args) {
    Student myObj = new Student();
    System.out.println("Name: " + myObj.fname + " " + myObj.lname);
    System.out.println("Email: " + myObj.email);
    System.out.println("Age: " + myObj.age);
    System.out.println("Graduation Year: " + myObj.graduationYear);
  }
}
```

%%
[[000 Indice POO|↖ Ritorna all'indice ↖]]
%%