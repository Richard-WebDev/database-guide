# 07 — Viste

[← Capitolo precedente](06-algebra-relazionale.md) · [Indice](README.md)

---

## Cos'è una vista

Una **vista** è una relazione **virtuale** derivata da altre relazioni del database tramite un'interrogazione (query).

È virtuale perché non contiene dati propri salvati su disco: ogni volta che la usi, il DBMS esegue la query sottostante e restituisce il risultato aggiornato.

Le viste sono lo strumento con cui si realizza il **livello esterno** visto nel capitolo 02: ogni utente vede solo la parte di dati che gli serve.

Esempio: una vista che mostra nome e reparto degli impiegati ma non lo stipendio. Con l'algebra del capitolo precedente:

```
π Cognome, Reparto (Impiegati)
```

---

## A cosa servono

1. **Sicurezza e controllo degli accessi**: si mostra a ciascun utente solo la porzione di dati che ha il diritto di vedere. Lo studente vede solo i propri voti, non quelli degli altri.
2. **Semplificazione delle interrogazioni**: una query complessa usata spesso si salva come vista e si usa come una semplice tabella.

---

## Vantaggi

- **Sicurezza**: si nascondono i dati sensibili. Il reparto HR vede solo nomi e reparti, non gli stipendi.
- **Semplicità**: una query con JOIN e molte condizioni diventa una tabella da consultare. Invece di riscrivere venti righe di SQL, basta `SELECT * FROM VistaVendite`.
- **Indipendenza logica**: se si ristruttura il database, si aggiorna la sola vista e le applicazioni che la usano non si accorgono di nulla.
- **Sempre aggiornata**: essendo virtuale, riflette i dati reali in tempo reale. Non si rischia di lavorare con dati vecchi.

## Svantaggi

- **Prestazioni**: a ogni uso il DBMS riesegue la query sottostante da zero. Se la query è complessa può essere lenta. È il prezzo dell'essere sempre aggiornata.
- **Modifica limitata**: in generale non si possono fare `INSERT`, `UPDATE` o `DELETE` attraverso una vista. Fanno eccezione alcune viste molto semplici, costruite su una sola tabella; dipende dal DBMS.
- **Dipendenza dalle tabelle originali**: se si elimina una tabella su cui si basa la vista, la vista smette di funzionare.

---

## In sintesi

Usa una vista quando servono **sicurezza e semplicità**. Quando le prestazioni sono critiche, valuta di sostituirla con una **vista materializzata**: una vista il cui risultato viene salvato fisicamente e aggiornato periodicamente, a costo di non essere sempre perfettamente allineata ai dati.

---

[← Capitolo precedente](06-algebra-relazionale.md) · [Indice](README.md)
