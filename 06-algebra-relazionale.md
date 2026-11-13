# 06 — Algebra relazionale

[← Capitolo precedente](05-integrita.md) · [Indice](README.md) · [Capitolo successivo →](07-viste.md)

---

## Cos'è

L'**algebra relazionale** è un linguaggio **procedurale** basato su concetti algebrici. È composto da un insieme di operatori definiti su relazioni, che producono a loro volta relazioni come risultato. Per questo gli operatori si possono **combinare** uno dentro l'altro.

*Procedurale* significa che si indica **come** ottenere il risultato, passo per passo. SQL, invece, è *dichiarativo*: si indica **cosa** si vuole, non come ottenerlo.

Per tutto il capitolo useremo queste relazioni di esempio:

**Impiegati**

| Matr | Cognome | Città | Reparto | Stipendio |
|---|---|---|---|---|
| 1 | Rossi | Milano | 10 | 1800 |
| 2 | Bianchi | Roma | 20 | 2200 |
| 3 | Verdi | Milano | 20 | 1500 |
| 4 | Neri | Torino | NULL | 1900 |

**Reparti**

| Reparto | Nome |
|---|---|
| 10 | Vendite |
| 20 | Acquisti |
| 30 | Ricerca |

---

## Operatori tradizionali (dagli insiemi)

Unione, intersezione e differenza richiedono che le due relazioni abbiano **gli stessi attributi** (stesso grado e domini compatibili).

| Operatore | Sintassi | Risultato |
|---|---|---|
| Unione | `R1 ∪ R2` | tutte le tuple di R1 **o** di R2, senza duplicati |
| Intersezione | `R1 ∩ R2` | le tuple presenti in **entrambe** |
| Differenza | `R1 − R2` | le tuple di R1 che **non** sono in R2 |
| Prodotto cartesiano | `R1 × R2` | tutte le combinazioni possibili tra tuple di R1 e tuple di R2 |

Esempi dagli appunti:

```
Laureati ∪ Specialisti     tutte le persone che sono laureate o specialiste (o entrambe)
Laureati ∩ Specialisti     le persone che sono sia laureate sia specialiste
Laureati − Specialisti     i laureati che non sono specialisti
Studenti × Esami           ogni studente combinato con ogni esame
```

Il prodotto cartesiano da solo produce molte combinazioni prive di senso: di solito è seguito da una selezione, ed è proprio ciò che fa il JOIN.

---

## Operatori specifici del modello relazionale

### Ridenominazione ρ

Rinomina gli attributi di una relazione. Serve, ad esempio, per fare un'unione tra due relazioni che rappresentano la stessa cosa ma con attributi di nome diverso.

```
ρ NuovoNome ← VecchioNome (Tabella)
```

Esempio: `Paternità(Padre, Figlio)` e `Maternità(Madre, Figlio)`. Per ottenere tutti i genitori non si può fare l'unione, perché gli attributi `Padre` e `Madre` hanno nomi diversi. Si rinomina uno dei due:

```
Paternità ∪ ρ Padre ← Madre (Maternità)
```

Ora `Maternità` ha l'attributo `Padre` al posto di `Madre` e l'unione è possibile. Il nome `Padre` è convenzionale; conviene in pratica usare un nome neutro come `Genitore` per entrambe.

### Selezione σ

Produce una relazione con **solo le tuple che soddisfano una condizione**. Taglia la tabella *orizzontalmente*: stesse colonne, meno righe (decomposizione orizzontale).

```
σ condizione (Tabella)
σ Città = 'Milano' (Impiegati)
```

| Matr | Cognome | Città | Reparto | Stipendio |
|---|---|---|---|---|
| 1 | Rossi | Milano | 10 | 1800 |
| 3 | Verdi | Milano | 20 | 1500 |

### Proiezione π

Produce una relazione con **solo gli attributi indicati**. Taglia la tabella *verticalmente*: stesse righe, meno colonne (decomposizione verticale). Se dopo la proiezione compaiono righe uguali, i duplicati vengono eliminati (è un insieme).

```
π attributi (Tabella)
π Cognome, Città (Impiegati)
```

| Cognome | Città |
|---|---|
| Rossi | Milano |
| Bianchi | Roma |
| Verdi | Milano |
| Neri | Torino |

### Selezione e proiezione insieme

Nella pratica si combinano. Voglio solo il cognome degli impiegati di Milano:

```
π Cognome ( σ Città = 'Milano' (Impiegati) )
```

Si legge dall'interno verso l'esterno: prima si selezionano le righe, poi si tengono le colonne volute.

| Cognome |
|---|
| Rossi |
| Verdi |

---

## JOIN

Il **JOIN** è l'operatore più importante dell'algebra relazionale: combina informazioni di due tabelle in una sola relazione.

Esiste perché nel modello relazionale i dati sono divisi in più tabelle per evitare ridondanza. Quando serve un'informazione completa che coinvolge più tabelle, vanno ricombinate: è il compito del JOIN.

### JOIN naturale ⨝

Unisce due relazioni usando gli attributi che hanno **lo stesso nome**. L'attributo comune compare una sola volta nel risultato.

```
R1 ⨝ R2
Impiegati ⨝ Reparti
```

Il collegamento avviene su `Reparto`:

| Matr | Cognome | Città | Reparto | Stipendio | Nome |
|---|---|---|---|---|---|
| 1 | Rossi | Milano | 10 | 1800 | Vendite |
| 2 | Bianchi | Roma | 20 | 2200 | Acquisti |
| 3 | Verdi | Milano | 20 | 1500 | Acquisti |

Notare cosa manca: `Neri` (reparto `NULL`, nessuna corrispondenza) e il reparto `Ricerca` (nessun impiegato).

Un JOIN può essere:

- **completo**: ogni tupla di R1 trova almeno una corrispondenza in R2 e viceversa, quindi nessuna tupla viene persa;
- **incompleto**: alcune tuple non trovano corrispondenza e **vengono escluse** dal risultato. È il caso qui sopra, e il problema è che **si perdono dati**.

### JOIN esterno (outer join)

Risolve il problema del JOIN incompleto: **conserva** anche le tuple senza corrispondenza, riempiendo con `NULL` i valori mancanti. Ha tre varianti.

| Variante | Sintassi | Conserva |
|---|---|---|
| **LEFT JOIN** | `R1 ⟕ R2` | tutte le tuple di R1 (a sinistra) |
| **RIGHT JOIN** | `R1 ⟖ R2` | tutte le tuple di R2 (a destra) |
| **FULL OUTER JOIN** | `R1 ⟗ R2` | tutte le tuple di entrambe |

`Impiegati ⟕ Reparti` aggiunge ai tre risultati precedenti anche Neri:

| Matr | Cognome | Città | Reparto | Stipendio | Nome |
|---|---|---|---|---|---|
| 1 | Rossi | Milano | 10 | 1800 | Vendite |
| 2 | Bianchi | Roma | 20 | 2200 | Acquisti |
| 3 | Verdi | Milano | 20 | 1500 | Acquisti |
| 4 | Neri | Torino | NULL | 1900 | NULL |

`Impiegati ⟖ Reparti` aggiungerebbe invece il reparto Ricerca (con i campi dell'impiegato a `NULL`), mentre `Impiegati ⟗ Reparti` conserverebbe entrambe le righe orfane.

### Theta-JOIN (θ-JOIN)

Unisce due relazioni in base a una **condizione esplicita**, non necessariamente l'uguaglianza tra attributi con lo stesso nome.

```
R1 ⨝(condizione) R2
```

Esempio: unire `Impiegati` e `Supervisori` dove lo stipendio dell'impiegato è minore di quello del supervisore:

```
Impiegati ⨝(Stipendio < StipendioSup) Supervisori
```

### EQUI-JOIN

Quando la condizione del theta-join è un'**uguaglianza**, si chiama equi-join.

```
R1 ⨝(R1.attributo = R2.attributo) R2
R1 ⨝(R1.Id = R2.Id) R2
```

Il JOIN naturale è un caso speciale di equi-join, in cui l'uguaglianza riguarda gli attributi con lo stesso nome (e l'attributo comune compare una sola volta).

---

## Riepilogo operatori

| Operatore | Simbolo | Sintassi | Effetto |
|---|---|---|---|
| Selezione | σ | `σ condizione (T)` | filtra le righe |
| Proiezione | π | `π attributi (T)` | tiene alcune colonne |
| Unione | ∪ | `R1 ∪ R2` | tutte le tuple di entrambe |
| Intersezione | ∩ | `R1 ∩ R2` | le tuple comuni |
| Differenza | − | `R1 − R2` | tuple di R1 non in R2 |
| Prodotto cartesiano | × | `R1 × R2` | tutte le combinazioni |
| Ridenominazione | ρ | `ρ Nuovo ← Vecchio (T)` | rinomina un attributo |
| JOIN naturale | ⨝ | `R1 ⨝ R2` | unisce sugli attributi omonimi |
| LEFT / RIGHT / FULL | ⟕ ⟖ ⟗ | `R1 ⟕ R2` | join che conserva le tuple senza corrispondenza |
| Theta-JOIN | ⨝(c) | `R1 ⨝(cond) R2` | join su condizione qualsiasi |
| Equi-JOIN | ⨝(=) | `R1 ⨝(R1.a = R2.a) R2` | theta-join con uguaglianza |

---

[← Capitolo precedente](05-integrita.md) · [Indice](README.md) · [Capitolo successivo →](07-viste.md)
