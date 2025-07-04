# Product Requirements Document (PRD)

L'URL dei dati di input è: [https://firmereferendum.giustizia.it/referendum/api-portal/iniziativa/public?v=1751271726271](https://firmereferendum.giustizia.it/referendum/api-portal/iniziativa/public?v=1751271726271)

## 1. Introduzione

- **Scopo del documento:** Definire i requisiti per lo sviluppo di un sito web che visualizza informazioni estratte da un dataset JSON fornito, utilizzando un formato a card per una presentazione chiara e coinvolgente.
- **Pubblico di riferimento:** Sviluppatori web, designer UX/UI, stakeholder del progetto.

## 2. Obiettivi

- Fornire un'interfaccia utente intuitiva e facile da navigare.
- Presentare le informazioni chiave in modo chiaro e sintetico.
- Permettere agli utenti di esplorare le informazioni in dettaglio se lo desiderano.
- Ottimizzare il sito per la visualizzazione su diversi dispositivi (desktop, tablet, mobile).

## 3. Funzionalità

### Visualizzazione a Card

- Ogni elemento del dataset JSON deve essere rappresentato come una card separata.
- Le card devono essere disposte in un layout responsive (griglia o simile).
- Le card devono contenere un sottoinsieme delle informazioni più rilevanti (vedi "Informazioni da visualizzare nelle card").

### Dettagli Elemento

- Cliccando su una card, l'utente deve essere reindirizzato a una pagina di dettaglio specifica.
- La pagina di dettaglio deve visualizzare tutte le informazioni disponibili per quell'elemento del dataset JSON.

### Paginazione

- Implementare un sistema di paginazione per gestire un numero elevato di elementi.
- Visualizzare il numero di elementi totali, l'indice della pagina corrente e il numero di elementi per pagina.

### Ricerca/Filtro

- Fornire una barra di ricerca per consentire agli utenti di trovare elementi specifici in base a parole chiave.
- Implementare filtri basati su categorie o altri attributi rilevanti (es. `idDecCatIniziativa.nome`, `idDecStatoIniziativa.nome`).
- **Filtri dinamici:** I filtri si influenzano reciprocamente mostrando solo le opzioni disponibili in base alle altre selezioni attive.
- **URL persistenti:** I filtri attivi vengono riflessi nell'URL permettendo di condividere link con filtri preimpostati e di mantenere lo stato durante la navigazione.

### Ordinamento

- Consentire agli utenti di ordinare gli elementi in base a determinati criteri (es. data di apertura, numero di sostenitori, titolo).

### Pagine Aggiuntive

#### Pagina Info

- Una pagina dedicata che descrive il progetto, i suoi obiettivi e come utilizzare il sito.
- Deve includere informazioni sul dataset utilizzato e la fonte dei dati.
- Accessibile tramite link nel menu di navigazione principale.

#### Vista Tabellare

- Una pagina che presenta le iniziative in formato tabellare per una visualizzazione più compatta.
- Colonne della tabella:
  - **Titolo:** `titolo` (con link alla single page dell'iniziativa)
  - **Tipologia:** `idDecTipologiaIniziativa.nome`
  - **Categoria:** `idDecCatIniziativa.nome`
  - **Stato:** `idDecStatoIniziativa.nome`
  - **Numero Sostenitori:** `sostenitori`
  - **Quorum:** `quorum`
  - **Data Apertura:** `dataApertura`
- La tabella deve essere ordinabile per ogni colonna con indicazione visiva della direzione di ordinamento.
- **Filtri avanzati:** Include la stessa logica di filtri della home page con:
  - Filtri dinamici che si influenzano reciprocamente
  - URL che si adatta ai filtri applicati
  - Barra di ricerca integrata
  - Pulsante per cancellare tutti i filtri
  - Contatore di risultati filtrati
- **Gestione stato vuoto:** Messaggio informativo quando non ci sono risultati con opzione per rimuovere i filtri.
- Accessibile tramite link nel menu di navigazione principale.

### Informazioni da visualizzare nelle card

- **Titolo:** `titolo`
- **Descrizione Breve:** `descrizioneBreve`
- **Categoria:** `idDecCatIniziativa.nome` (es. "VITA POLITICA")
- **Stato:** `idDecStatoIniziativa.nome` (es. "IN RACCOLTA FIRME")
- **Data Apertura:** `dataApertura` (formattata in modo leggibile)
- **Numero Sostenitori:** `sostenitori`

### Informazioni da visualizzare nella pagina di dettaglio

- Tutti i campi del JSON, formattati in modo leggibile.
- Possibilità di tornare alla pagina principale.

### Link Esterni

- Se presente, il campo `sito` deve essere visualizzato come un link cliccabile che apre una nuova scheda.

### Funzionalità di Condivisione

- Nella pagina di dettaglio, accanto al tasto "Sostieni", deve essere presente un tasto "Condividi".
- Il tasto "Condividi" deve permettere la condivisione dell'URL della pagina su social media, chat e altre piattaforme.
- La funzionalità deve utilizzare la Web Share API nativa quando disponibile sui dispositivi mobili.
- Come fallback per desktop, deve fornire opzioni di condivisione per le principali piattaforme (WhatsApp, Telegram, Twitter, Facebook, LinkedIn).
- L'URL condiviso deve includere i metadati OpenGraph ottimizzati per una preview ricca sui social.

## 4. Wireframes/Mockups

- **Pagina Principale:**
  - Header: Logo del sito, barra di ricerca, filtri, ordinamento.
  - Area card: Griglia di card, paginazione.
- **Pagina di Dettaglio:**
  - Header: Titolo dell'elemento.
  - Contenuto: Visualizzazione formattata di tutti i campi del JSON.
  - Footer: Link per tornare alla pagina principale.

## 5. Design (UI)

- **Schema colori:** Palette moderna e pulita utilizzando Tailwind CSS
- **Tipografia:** Font system di Tailwind CSS per leggibilità ottimale
- **Stile delle card:** Design minimalista con ombre e bordi arrotondati
- **Responsività:** Il sito deve essere pienamente responsive e adattabile a diverse dimensioni di schermo utilizzando Tailwind CSS

## 6. Dati

- **Formato dati:** JSON (come fornito dall'API)
- **Origine dati:** API endpoint fornito dal Ministero di Giustizia
- **Aggiornamento dati:** I dati vengono recuperati al momento del build del sito statico

## 7. Requisiti Tecnici

- Il sito deve essere statico: per ogni referendum o proposta di legge verrà generata una pagina singola (single page) dedicata.
- **Astro** è il framework scelto per la generazione statica delle pagine e la gestione delle rotte.

- Ogni single page avrà una sezione OpenGraph ottimizzata per la condivisione su social e chat:
  - Verrà generata staticamente un'immagine di preview con il titolo dell'iniziativa.
  - Il link OpenGraph non punterà alla pagina web del sito, ma direttamente alla pagina ufficiale del Referendum o dell'iniziativa popolare, così l'utente potrà partecipare subito.

- **Linguaggi e Framework:**
  - HTML, CSS, JavaScript
  - Framework: **Astro** per la generazione statica e la gestione delle pagine
  - Componenti: Astro components con possibile integrazione di componenti React/Vue se necessario
- **Responsività:** Utilizzo di Tailwind CSS per garantire la responsività e un design moderno.
- **Paginazione:** Implementazione client-side per la navigazione tra le card.
- **Hosting:** Il sito sarà ospitato su **GitHub Pages** con supporto per siti statici Astro.
- **SEO:**
  - Ottimizzazione per i motori di ricerca (meta description, titoli, URL amichevoli).
  - Ottimizzazione OpenGraph per ogni single page, con immagine di anteprima e link diretto alla pagina ufficiale dell'iniziativa.
  - Astro genera automaticamente HTML ottimizzato per SEO.

## 8. Test

- **Test di funzionalità:** Verificare che tutte le funzionalità funzionino correttamente.
- **Test di usabilità:** Assicurarsi che l'interfaccia sia intuitiva e facile da usare.
- **Test di compatibilità:** Verificare la compatibilità con diversi browser e dispositivi.
- **Test di performance:** Assicurarsi che il sito si carichi rapidamente e sia reattivo.

## 9. Criteri di Accettazione

- Tutte le funzionalità devono essere implementate secondo le specifiche.
- Il design deve corrispondere ai mockup approvati.
- Il sito deve superare tutti i test.
- Il codice deve essere pulito, ben documentato e conforme agli standard di codifica.

## 10. Tempi e Budget

- (Da definire)
