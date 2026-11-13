# 04 — Il modello relazionale

[← Capitolo precedente](03-insiemi.md) · [Indice](README.md) · [Capitolo successivo →](05-integrita.md)

---

## Relazione matematica

Una **relazione matematica** è un **sottoinsieme di un prodotto cartesiano** tra due o più insiemi (domini).

Si definisce prima il prodotto cartesiano, poi si scelgono solo le tuple che hanno senso per il problema: non devono esserci tutte le combinazioni possibili.

Due termini da ricordare:

- **Grado**: numero di domini (colonne) coinvolti nel prodotto cartesiano.
- **Cardinalità**: numero di tuple (righe) presenti nella relazione.

---

## Relazione nel modello relazionale

Nella relazione matematica i domini sono anonimi e contano per posizione. Nel modello relazionale ogni dominio riceve un **nome**, detto **attributo**, e i valori si identificano per nome, non più per posizione. Per questo l'ordine delle colonne non conta.

- **Attributo**: il nome di una colonna. Rappresenta il ruolo che quel dominio gioca nella relazione.
- **Tupla**: una riga della tabella. È una funzione che assegna a ogni attributo un valore del suo dominio.

Esempio, i risultati di alcune partite:

| Casa | Fuori | RetiCasa | RetiFuori |
|---|---|---|---|
| Juve | Lazio | 3 | 1 |
| Lazio | Milan | 2 | 0 |
| Milan | Roma | 1 | 1 |
| Roma | Juve | 0 | 2 |

- **Attributi**: Casa, Fuori, RetiCasa, RetiFuori
- **Tupla**: ogni riga, ad esempio `(Juve, Lazio, 3, 1)`
- **Grado**: 4 (numero di attributi)
- **Cardinalità**: 4 (numero di tuple)

---

## Schema di relazione

Lo **schema di relazione** è la struttura di una tabella: nome della relazione ed elenco degli attributi, con i relativi domini.

```
Studenti(Matricola, Cognome, Nome, DataNascita)
```

Con i domini:

```
Studenti(Matricola: intero, Cognome: stringa, Nome: stringa, DataNascita: data)
```

### Schema di database relazionale

Lo schema di relazione descrive una tabella. Lo **schema di database** descrive **tutte** le tabelle e come sono collegate.

```
Studenti(Matricola, Cognome, Nome, DataNascita)
Esami(Matricola, Materia, Voto, Data)
```

> **Nota bene:** l'attributo `Matricola` compare sia in `Studenti` sia in `Esami`. Non è un caso: è il collegamento tra le due tabelle. Per sapere quali esami ha sostenuto lo studente 6554, si cercano in `Esami` le righe con `Matricola = 6554`. Questo meccanismo si chiama **chiave esterna** (vedi più avanti).

---

## Istanza di relazione e di database

Come per il database in generale (capitolo 01), esiste la distinzione schema/istanza.

- **Istanza di relazione**: l'insieme delle tuple presenti in una singola tabella *in un determinato momento*. Cambia a ogni inserimento, modifica o cancellazione.
- **Istanza di database**: l'insieme di tutte le istanze di tutte le relazioni del database in un determinato momento.

---

## Esempio: le ricevute fiscali

Come rappresentare delle ricevute con il modello relazionale? Una ricevuta ha dati propri (numero, data, totale) e più righe di dettaglio (prodotto, quantità, prezzo).

**Primo tentativo: una sola tabella**

| Numero | Data | Totale | Prodotto | Quantità | Prezzo |
|---|---|---|---|---|---|
| 1 | 10/05 | 12,00 | Pane | 2 | 2,00 |
| 1 | 10/05 | 12,00 | Latte | 4 | 2,00 |
| 2 | 11/05 | 3,50 | Caffè | 1 | 3,50 |

Numero, Data e Totale della ricevuta 1 si ripetono per ogni riga di dettaglio. È **ridondanza**, proprio il problema che il database deve evitare.

**Soluzione: due relazioni collegate**

```
Ricevute(Numero, Data, Totale)
Dettaglio(Numero, Prodotto, Quantità, Prezzo)
```

| Numero | Data | Totale |
|---|---|---|
| 1 | 10/05 | 12,00 |
| 2 | 11/05 | 3,50 |

| Numero | Prodotto | Quantità | Prezzo |
|---|---|---|---|
| 1 | Pane | 2 | 2,00 |
| 1 | Latte | 4 | 2,00 |
| 2 | Caffè | 1 | 3,50 |

Regola pratica: se gli stessi dati si ripetono su più righe, stai mettendo troppo in una sola tabella. Si divide in più tabelle e si collegano con un attributo comune.

---

## Superchiave

Una **superchiave** è un insieme di uno o più attributi che identifica in modo univoco ogni tupla di una tabella.

Con `Studente(Matricola, CodiceFiscale, Cognome, Nome)`:

1. `{Matricola}` è una superchiave: data una matricola, esiste un solo studente.
2. `{CodiceFiscale}` è una superchiave: ogni persona ha un codice diverso.
3. `{Matricola, Cognome}` è una superchiave: se la matricola basta già, aggiungere il cognome non rompe l'univocità.

---

## Chiave

In un database relazionale non possono esistere due righe identiche: deve esserci sempre un modo per distinguerle.

Una **chiave** (più correttamente *chiave candidata*) è una superchiave che rispetta la **minimalità**:

1. identifica univocamente le tuple (è una superchiave);
2. è **minimale**: togliendo anche un solo attributo smette di essere identificatore.

Nell'esempio precedente:

- `{Matricola, Cognome}` è una superchiave ma **non** una chiave: togliendo `Cognome`, la `Matricola` basta ancora.
- `{Matricola}` è una chiave.
- `{CodiceFiscale}` è un'altra chiave.

In sintesi: **superchiave** = identificatore (può avere attributi in più); **chiave** = identificatore minimo.

---

## Chiave primaria (Primary Key)

La **chiave primaria** è la chiave che scegliamo ufficialmente per distinguere ogni riga. Deve rispettare tre regole:

1. **Univocità**: non esistono due righe con la stessa chiave primaria.
2. **Minimalità**: è composta dal minor numero possibile di attributi.
3. **Obbligatorietà**: non può mai essere `NULL`. Se non so chi sei (niente matricola), non posso inserirti.

Nella notazione dello schema la chiave primaria si **sottolinea**.

Perché "primaria"? Una tabella può avere più chiavi candidate (Matricola e CodiceFiscale). Ne scegliamo una come primaria perché sarà quella usata per collegare la tabella alle altre.

---

## Chiave esterna (Foreign Key)

Le tabelle si collegano con attributi comuni. Come garantire che il collegamento sia sempre valido?

Una **chiave esterna** è un attributo (o un insieme di attributi) di una tabella A che fa riferimento alla **chiave primaria** di una tabella B.

Non è solo un collegamento: è un **vincolo di integrità referenziale**. Il database controlla che non si inseriscano valori inventati.

Esempio con `Studenti(Matricola, …)` e `PrestitiBiblioteca(Libro, MatricolaStudente, …)`:

`MatricolaStudente` è una chiave esterna verso `Studenti.Matricola`. Se provi a registrare un prestito per la matricola 999 e in `Studenti` la matricola 999 non esiste, il database blocca l'operazione.

---

## Valori NULL

`NULL` non è un valore nel senso tradizionale (non è zero, né una stringa vuota): è un **segnaposto** che indica l'**assenza** di un valore nel dominio dell'attributo.

Può significare dato mancante, sconosciuto o non applicabile. Per questo:

- gli attributi che formano la chiave primaria non possono mai essere `NULL`;
- per vietare il `NULL` su altri attributi si usa il vincolo `NOT NULL`;
- i `NULL` vanno usati con parsimonia: rendono ambigue le interrogazioni, perché un confronto con un `NULL` non è né vero né falso ma *sconosciuto*.

Esempio: in una tabella `Studenti` con una riga `(NULL, Rossi, Mario)` la matricola mancante rende la riga non identificabile. Quella riga non dovrebbe poter esistere.

---

[← Capitolo precedente](03-insiemi.md) · [Indice](README.md) · [Capitolo successivo →](05-integrita.md)
