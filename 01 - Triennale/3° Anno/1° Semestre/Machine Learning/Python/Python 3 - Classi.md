---
tags:
  - corsi/informatica/machine_learning
---
Le [[Classi]] in Python sono un meccanismo fondamentale della programmazione orientata agli oggetti (OOP). Una classe rappresenta un modello o un prototipo da cui vengono creati gli oggetti.

```python
class Persona:
    def __init__(self, nome, età):
        self.nome = nome
        self.età = età

    def saluta(self):
        print(f"Ciao, sono {self.nome}!")


# Creazione di un oggetto
persona1 = Persona("Mario", 30)

print(persona1.nome)  # Output: Mario
print(persona1.età)   # Output: 30
persona1.saluta()     # Output: Ciao, sono Mario!
```

Nell'esempio sopra:
- `__init__` è un metodo speciale chiamato [[Costruttore di Oggetti|Costruttore]]. Viene eseguito automaticamente quando un oggetto della classe viene creato. 
	- `self` rappresenta l'istanza della classe e viene utilizzato per accedere alle variabili della classe.
- `saluta` è un [[Metodi|Metodo]] della classe. I metodi sono funzioni definite all'interno della classe e operano sugli oggetti creati da quella classe.

### [[015 Ereditarietà|Ereditarietà]]
```python
class Studente(Persona):
    def __init__(self, nome, età, corso):
        super().__init__(nome, età)
        self.corso = corso

    def studia(self):
        print(f"{self.nome} sta studiando {self.corso}.")
```

### [[016 Polimorfismo|Polimorfismo]]
```python
def saluto_generico(persona):
    persona.saluta()

saluto_generico(persona1)      # Output: Ciao, sono Mario!
saluto_generico(studente1)     # Output: Ciao, sono Luca!
```


%%
[[000 Indice ML|↖ Ritorna all'indice ↖]]
%%