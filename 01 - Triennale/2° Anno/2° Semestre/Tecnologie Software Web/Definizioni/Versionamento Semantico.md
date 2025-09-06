---
tags:
  - corsi/informatica/tecnologie_software_web
---
Esiste uno standard del settore denominato _versionamento semantico_. Il versionamento semantico consente di esprimere il tipo di modifica che uno sviluppatore introduce in una libreria. Il controllo delle versioni semantico assicura che un pacchetto abbia un numero di versione e che il numero di versione sia suddiviso nelle sezioni seguenti:

- **Versione principale**: il numero più a sinistra. In 1.0.0, ad esempio, è 1. Una modifica a questo numero indica che è possibile prevedere modifiche che causano interruzioni nel codice e potrebbe essere necessario riscrivere parte del codice.
- **Versione secondaria**: il numero intermedio. In 1.2.0, ad esempio, è 2. Una modifica a questo numero indica che sono state aggiunte funzionalità. Il codice dovrebbe continuare a funzionare. È in genere sicuro accettare l'aggiornamento.
- **Versione patch**: il numero più a destra. In 1.2.3, ad esempio, è 3. Una modifica apportata a questo numero indica che è stata applicata una modifica che corregge una parte del codice non funzionante. È in genere sicuro accettare l'aggiornamento.

Questa tabella illustra come cambia il numero di versione per ogni tipo di versione:

|Tipo|Risultato finale|
|---|---|
|Versione principale|1.0.0 diventa 2.0.0|
|Versione secondaria|1.1.1 diventa 1.2.0|
|Versione della patch|1.0.1 diventa 1.0.2|


[Da Microsoft](https://learn.microsoft.com/it-it/training/modules/dotnet-dependencies/4-dependency-management)

%%
[[000 Indice TSW|↖ Ritorna all'indice ↖]]
%%