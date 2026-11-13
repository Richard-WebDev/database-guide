# 02 — Astrazione e linguaggi

[← Capitolo precedente](01-dati-e-database.md) · [Indice](README.md) · [Capitolo successivo →](03-insiemi.md)

---

## Livelli di astrazione

**Astrarre** significa nascondere i dettagli di un livello inferiore per semplificare il lavoro a quello superiore. Nel database ogni livello nasconde la complessità del livello sotto di sé. I livelli sono tre.

### 1. Livello esterno

È il più vicino all'utente. Ogni utente o applicazione vede solo la porzione di dati che gli interessa, non l'intero database.

> Lo studente vede solo i propri voti. Il professore vede i voti di tutti i suoi studenti. L'amministratore vede tutto. Stessi dati, viste diverse.

### 2. Livello logico

Descrive l'intera struttura del database attraverso il modello logico (in questa guida, il modello relazionale con le sue tabelle). Progettisti e programmatori lavorano soprattutto qui.

### 3. Livello fisico

Descrive come i dati sono salvati su disco: file, indici, strutture di memorizzazione. Lo gestisce internamente il DBMS.

---

## Indipendenza dei dati

La separazione in tre livelli garantisce una proprietà fondamentale del DBMS, l'**indipendenza dei dati**, di due tipi:

- **Indipendenza fisica**: se cambia il modo in cui i dati sono salvati su disco (livello fisico), il livello logico non cambia e i programmi continuano a funzionare.
- **Indipendenza logica**: se cambia la struttura logica del database (livello logico), le viste degli utenti (livello esterno) non devono necessariamente cambiare.

---

## I componenti del linguaggio: DDL, DML, DQL

Ogni DBMS mette a disposizione un linguaggio con componenti distinte, ciascuna con un compito preciso.

| Sigla | Nome | A cosa serve |
|---|---|---|
| **DDL** | Data Definition Language | definire la struttura (lo **schema**): tabelle, attributi, vincoli. Non tocca i dati |
| **DML** | Data Manipulation Language | manipolare i dati (le **istanze**): inserire, modificare, cancellare |
| **DQL** | Data Query Language | interrogare il database, cioè leggere i dati senza modificarli |

> In molti testi il DQL è considerato parte del DML. Qui si tiene separato per chiarezza.

---

## Tipologie di linguaggi

Utenti diversi hanno competenze e necessità diverse, quindi esistono diversi modi di interagire con un database.

- **Linguaggi interattivi** (es. SQL): permettono di dialogare direttamente con il database in tempo reale, scrivendo comandi da tastiera o tramite un'interfaccia grafica.
- **Linguaggi ospiti** (C, Java, PHP, Python…): linguaggi di programmazione tradizionali dentro i quali vengono incorporati comandi SQL. Si chiamano "ospiti" perché ospitano le istruzioni SQL.
- **Linguaggi ad hoc**: progettati per uno specifico DBMS o applicazione, con una sintassi propria pensata per quel contesto.
- **Interfacce grafiche**: non sono veri linguaggi testuali. Il database offre un'interfaccia con cui l'utente lavora sui dati senza scrivere codice.

Esempio di linguaggio ospite, PHP che esegue una query SQL:

```php
<?php
$stmt = $pdo->prepare("SELECT Cognome, Nome FROM Studenti WHERE Matricola = ?");
$stmt->execute([1001]);
$studente = $stmt->fetch();
```

---

## Gli utenti di un database

Un database non è usato da un solo tipo di persona. Le figure hanno competenze, responsabilità e permessi diversi.

- **Amministratore (DBA)**: la figura più importante, con controllo totale. Decide chi può accedere al database e con quali permessi.
- **Progettista**: disegna la struttura del database (tabelle, attributi, collegamenti). Lavora a livello logico, usando il DDL.
- **Programmatore**: sviluppa le applicazioni che usano il database, in Java, PHP, C… Usa i linguaggi ospiti.
- **Utenti finali**: usano il database tramite le applicazioni dei programmatori. Non conoscono SQL né la struttura interna.
- **Utenti casuali**: accedono in modo occasionale e imprevedibile, con esigenze diverse ogni volta. L'amministratore dà loro permessi specifici e limitati. Esempio: un revisore esterno che accede solo durante un audit annuale, con permessi di sola lettura su alcune tabelle.

---

## Vantaggi e svantaggi di un DBMS

### Vantaggi

- **Dati come risorsa condivisa**: scritti una volta sola, accessibili a tutte le applicazioni e gli utenti autorizzati. Niente copie sparse che si contraddicono.
- **Linguaggio standard**: tutti comunicano con il database con lo stesso linguaggio (SQL), qualunque sia il linguaggio dell'applicazione.
- **Sicurezza e controllo degli accessi**: ogni utente vede solo i dati autorizzati, con il permesso adeguato (sola lettura, lettura e scrittura…).
- **Indipendenza dei dati**: le applicazioni non dipendono da come i dati sono memorizzati fisicamente.
- **Integrità e consistenza**: i dati rispettano sempre le regole definite (i vincoli). Non puoi inserire un voto di 50 dove il voto deve stare tra 18 e 30.
- **Affidabilità e ripristino**: dopo un guasto il DBMS riporta i dati all'ultimo stato consistente, grazie alle **transazioni**.

### Svantaggi

- **Costo**: un DBMS professionale (es. Oracle, SQL Server) ha costi di licenza, installazione e manutenzione alti, e richiede personale qualificato. Esistono però anche DBMS gratuiti e open source (es. MySQL, PostgreSQL).
- **Complessità**: è un sistema pensato per coprire ogni caso d'uso. Per un'applicazione semplice è un sistema enorme per un problema piccolo.
- **Overhead prestazionale**: sicurezza, vincoli e concorrenza hanno un costo computazionale. Per applicazioni molto semplici un file può essere più veloce.

---

[← Capitolo precedente](01-dati-e-database.md) · [Indice](README.md) · [Capitolo successivo →](03-insiemi.md)
