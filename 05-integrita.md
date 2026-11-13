# 05 — Vincoli di integrità

[← Capitolo precedente](04-modello-relazionale.md) · [Indice](README.md) · [Capitolo successivo →](06-algebra-relazionale.md)

---

## Cos'è un vincolo

Un database archivia dati, ma non tutti i dati sono corretti. Un voto d'esame pari a 150 è un dato, ma non è valido in un contesto universitario.

I **vincoli** sono regole che il DBMS controlla automaticamente per garantire che i dati inseriti siano sempre corretti e consistenti.

Formalmente, un vincolo è un **predicato** che associa a ogni istanza un valore di verità (vero o falso). Se l'istanza lo soddisfa, è considerata corretta.

---

## Tipi di vincoli

### Vincoli intra-relazionali

Il loro soddisfacimento è definito rispetto a **una sola relazione**.

- **Vincolo di dominio (o di valore)**: i valori di un attributo devono appartenere a un insieme di valori ammissibili. Esempio: il voto d'esame è compreso tra 18 e 30.
- **Vincolo di tupla**: una restrizione che riguarda **una riga alla volta**. È la forma più semplice di controllo. Esempio: `Voto >= 18 AND Voto <= 30`; ogni singola riga deve rispettarla.
- **Vincolo di chiave**: due righe non possono avere la stessa chiave. È il vincolo che garantisce l'univocità.
- **Vincolo NOT NULL**: l'attributo non può assumere il valore `NULL`.

### Vincoli inter-relazionali

Coinvolgono **più relazioni** e collegano in modo coerente tabelle diverse.

- **Chiave esterna (Foreign Key)**: il vincolo di integrità referenziale per eccellenza. Un valore presente in una tabella deve esistere nella tabella "padre". Esempio: la matricola in `Esami` deve esistere in `Studenti`; non puoi registrare un esame per uno studente inesistente.

---

## Integrità referenziale: un esempio

L'integrità referenziale garantisce che i collegamenti tra tabelle siano corretti: una chiave esterna deve sempre riferirsi a una chiave primaria esistente, senza riferimenti a elementi inesistenti.

Consideriamo tre relazioni sulle multe stradali:

```
Agente(Matricola, Cognome, Nome)
Auto(Stato, Targa, Proprietario)
Infrazione(Codice, Data, Agente, Stato, Targa)
```

**Vincolo tra Infrazione e Agente**: `Infrazione.Agente` è una chiave esterna verso `Agente.Matricola`.

- Se un'infrazione ha `Agente = 567`, deve esistere in `Agente` la matricola 567. Allora il vincolo è soddisfatto.
- Se si prova a inserire un'infrazione con `Agente = 999` e la matricola 999 non esiste, il sistema restituisce un errore.

**Vincolo tra Infrazione e Auto**: qui la chiave esterna è formata da **due attributi**, `(Stato, Targa)`, e punta alla chiave primaria composta di `Auto`. Un'infrazione con `Stato = F` e `Targa = CB 123 AA` è valida solo se in `Auto` esiste esattamente quella coppia.

Senza questi controlli il database darebbe dati incompleti senza segnalare errori, con anomalie gravi, ad esempio un'infrazione legata a un'auto che non esiste più nel sistema.

---

## Modifica o eliminazione di un record referenziato

Con un vincolo di integrità referenziale, non si può semplicemente eliminare o modificare un record "padre" se esistono "figli" che lo referenziano. Ma nella realtà i dati vanno pur modificati o eliminati. Il DBMS offre **quattro strategie**, scelte dal progettista in fase di creazione della tabella.

| Strategia | Cosa succede ai record figli | Quando ha senso |
|---|---|---|
| **NO ACTION** | l'operazione viene **bloccata** con un errore | quando i dati collegati non devono mai restare orfani né sparire |
| **CASCADE** | l'operazione si **propaga**: le righe figlie vengono eliminate (o aggiornate) automaticamente | quando i figli non hanno senso senza il padre, es. le righe di dettaglio di una ricevuta |
| **SET NULL** | la chiave esterna dei figli viene impostata a `NULL`: il collegamento si rompe ma le righe restano | quando i figli vanno conservati anche senza padre; richiede che l'attributo ammetta `NULL` |
| **SET DEFAULT** | la chiave esterna dei figli viene impostata a un valore predefinito | quando esiste un padre "di ripiego"; il valore di default deve essere già definito e valido |

### Come scegliere

Esempio: un agente va in pensione e il suo record viene eliminato.

- Con **CASCADE** sparirebbero anche tutte le sue infrazioni: quasi sicuramente non è ciò che si vuole, perché lo storico delle multe va conservato.
- Con **SET NULL** le infrazioni restano, solo senza agente assegnato.
- Con **NO ACTION** l'eliminazione dell'agente viene rifiutata finché esistono infrazioni a suo nome.

Invece per `Ricevute` e `Dettaglio` del capitolo precedente **CASCADE** è la scelta naturale: se si elimina una ricevuta, le sue righe di dettaglio non servono più.

La strategia dipende quindi dal significato dei dati, non esiste una scelta migliore in assoluto.

---

## Definizione di record

Un **record** (o tupla) è una singola istanza di dati che rappresenta un elemento della realtà all'interno di una relazione (tabella).

---

[← Capitolo precedente](04-modello-relazionale.md) · [Indice](README.md) · [Capitolo successivo →](06-algebra-relazionale.md)
