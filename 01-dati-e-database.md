# 01 — Dati e database

[Indice](README.md) · [Capitolo successivo →](02-astrazione-linguaggi.md)

---

## Cos'è un dato?

Un **dato** è un fatto grezzo, un simbolo o un valore registrato, privo di contesto. Da solo non comunica nulla di utile.

Esempio: leggi `25`. È un dato. Isolato, non dice niente.

## Cos'è un'informazione?

L'**informazione** è ciò che si ottiene quando a un dato si associa una chiave di lettura, cioè un'interpretazione e un contesto.

`Voto esame di Matematica: 25` è un'informazione: sai cosa rappresenta il numero, in quale dominio si colloca (voti universitari, scala 18–30) e a cosa si riferisce.

---

## Cos'è un file?

Un **file** è una raccolta di dati archiviata su una memoria di massa (disco fisso, SSD), organizzata in modo che un programma specifico possa leggerla e scriverla.

### Il problema dei file

Quando i dati crescono o devono essere condivisi, i file mostrano limiti gravi:

- **Ridondanza**: gli stessi dati sono copiati in più file. Se uno studente cambia cognome, va aggiornato a mano in ciascuno.
- **Inconsistenza**: se dimentichi di aggiornare un file, in giro ci sono dati contraddittori.
- **Accesso concorrente problematico**: se due persone modificano lo stesso file nello stesso momento, uno dei due rischia di sovrascrivere il lavoro dell'altro, fino alla perdita o alla corruzione dei dati.
- **Nessun controllo di sicurezza**: chi ha accesso al file può vedere e modificare tutto.

Questi limiti sono il motivo per cui sono stati inventati i database.

---

## Cos'è un database?

Un **database** (base di dati) è una raccolta di dati **grande, condivisa e persistente**, gestita in modo controllato e strutturato.

1. **Grande**: più grande della memoria centrale (RAM). Se la RAM non basta, serve un database.
2. **Condivisa**: accessibile a più persone o applicazioni nello stesso momento, anche per modificare i dati.
3. **Persistente**: i dati restano archiviati e disponibili oltre la durata dei programmi che li usano.

---

## Cos'è un DBMS?

Un **DBMS** (*DataBase Management System*) è il software che si interpone tra l'utente e il database, gestendo in modo controllato tutte le operazioni sui dati.

Un buon DBMS deve essere:

1. **Affidabile**: garantisce che i dati si ritrovino nel tempo e restino corretti, anche dopo un guasto.
2. **Sicuro (privatezza)**: i dati sono accessibili solo a chi è autorizzato, con i permessi appropriati.
3. **Efficiente**: risponde alle richieste nel minor tempo possibile, usando poche risorse.
4. **Efficace**: mette a disposizione tutte le funzionalità necessarie per gestire i dati correttamente.
5. **Indipendente dai dati**: separa la descrizione logica dei dati dalla loro memorizzazione fisica. Se cambia il modo in cui i dati sono salvati su disco, i programmi che li usano non vanno riscritti (vedi capitolo 02).

---

## Modelli di dati

Un **modello di dati** è un insieme di concetti usati per organizzare e descrivere i dati in modo che il DBMS possa gestirli. Ce ne sono di tre tipi.

### 1. Modelli concettuali

Servono nella fase di progettazione, quando ancora non si pensa alla tecnologia ma solo alla realtà da rappresentare. Sono legati all'utente e al mondo reale, non al computer.

Il più usato è il **modello E-R** (Entità-Relazione).

### 2. Modelli logici

Rappresentano i dati in modo che il DBMS possa gestirli, restando indipendenti dalla memorizzazione fisica. I principali:

| Modello | Come organizza i dati | Nota |
|---|---|---|
| **Gerarchico** | ad albero: un nodo radice, ogni padre ha più figli, ogni figlio ha **un solo** padre | non rappresenta relazioni in cui un figlio ha più padri |
| **Reticolare** | come un grafo: un figlio può avere **più padri** | supera il limite del gerarchico |
| **Relazionale** | in tabelle (relazioni) fatte di righe (tuple) e colonne (attributi) | il più usato oggi; è quello di questa guida |
| **A oggetti** | come oggetti della programmazione a oggetti, con attributi e metodi | stessa idea di Java |

### 3. Modelli fisici

Descrivono come i dati sono effettivamente salvati su disco: strutture di file, indici, puntatori. Li gestisce internamente il DBMS: progettista e programmatore normalmente non li vedono.

---

## Schema e istanza

Costruendo il database di una scuola, prima di inserire qualsiasi studente devi decidere com'è fatta la tabella: quante colonne ha, come si chiamano, che tipo di dati contengono. I dati degli studenti reali che ci metti dentro sono un'altra cosa.

| | Schema | Istanza |
|---|---|---|
| Altro nome | parte **intensionale** | parte **estensionale** |
| Cos'è | la struttura: tabelle, attributi, tipi | i dati reali presenti in un dato momento |
| Come cambia | quasi mai, solo se si riprogetta il database | continuamente, a ogni inserimento, modifica o cancellazione |

Esempio con una tabella `Studenti(Matricola, Cognome, Nome)`:

**Istanza prima**

| Matricola | Cognome | Nome |
|---|---|---|
| 1001 | Rossi | Mario |
| 1002 | Bianchi | Anna |

**Istanza dopo** (inserito uno studente e cambiato un cognome)

| Matricola | Cognome | Nome |
|---|---|---|
| 1001 | Rossi | Mario |
| 1002 | Verdi | Anna |
| 1003 | Neri | Luca |

L'istanza è cambiata, lo schema `Studenti(Matricola, Cognome, Nome)` no.

---

[Indice](README.md) · [Capitolo successivo →](02-astrazione-linguaggi.md)
