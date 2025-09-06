---
aliases:
  - FBC
  - i-node
tags:
  - corsi/informatica/sistemi_operativi
---
Un **File Control Block (FBC)** è una struttura nel file system in cui si conservano alcuni [[007 File#ATTRIBUTI DI UN FILE|attributi]] e lo stato attuale di un file.
In UNIX il FCB viene chiamato *i-node*.

Contiene:
- La *dimensione totale* del file in bytes o blocchi;
- Puntatore al *primo blocco* dati del file;
- I *permessi* del file;
- Le *date* di creazione, accesso e modifica del file;
- Il *proprietario* o gruppo del file.

%%[[000 Indice SO|↖ Ritorna all'indice ↖]]%%