#Elasticsearch

### Introduzione
Elasticsearch è un server di ricerca NRT (Near RealTime), ovvero con una piccola latenza (circa un secondo) tra il tempo in cui viene indicizzato un documento a quando diventa ricercabile. E' open source e sviluppato in Java ed è basato su **Lucene**, una potente libreria di ricerca full-text open source; Lucene è molto complesso da utilizzare e scrivere un'applicazione che ne faccia uso è molto complesso mentre Elasticsearch espone all'utente finale una semplice API RESTful astraendo la complessità interna e rendendo il tutto facilmente integrabile con client di numerosi linguaggi.  
ES è basato sugli indici che non sono altro che contenitori di uno o più documenti i quali a loro volta contengono uno o più campi; per capire meglio queste relazioni conviene vedere questo schema che affianca i termini di ES agli analoghi termini che conosciamo per i DB relazionali.  

DB relazionale  ⇒ Databases ⇒ Tabelle ⇒ Righe      ⇒ Colonne  
Elasticsearch  ⇒ Indici   ⇒ Tipi  ⇒ Documenti ⇒ Campi

### 3-6 Paragrafi di Relazione
#### Come è fatto
Elasticsearch è molto performante per le ricerche full-text; per ricerca full-text si intende un tipo di ricerca di testo in cui vengono esaminate tutte le parole in ogni documento memorizzato. Per ottenere queste performance, ES ha una struttura chiamata ad indice invertito (**inverted index**); in questo modo si passa dalle strutture classiche paginocentriche (in cui per ogni documento c'era la lista delle parole in esso contenute) a strutture parolechiavecentriche in cui per ogni parola unica presente nei documenti è associata la lista di documenti che la contengono.  
Un'altra caratteristica importante di Elasticsearch è il fatto di essere distribuito e scalabile fino a centinaia di server e petabyte di dati.  
La sua configurazione è composta da un cluster con uno o più nodi aventi lo stesso cluster.name (editabile dentro elasticsearch.yml) dove ogni nodo, che corrisponde ad una singola istanza di ES, è installato su un server o su una macchina virtuale.  
Allo stesso tempo è importante il concetto di **Shards** che non sono altro che singole parti in cui può esser diviso un indice (che avevamo visto corrisponde in terminologia relazionale ad un database). Ogni shard è praticamente un indice indipendente e con tutte le funzionalità e può esser mantenuto su qualsiasi nodo del cluster.  
**L'utilizzo degli shards è importante sia per la scalabilità orizzontale sia per distribuire e parallelizzare le operazioni.**  
L'architettura distribuita di ES è fatta in maniera trasparente all'utente in quanto sono fatte in automatico le seguenti operazioni:
* partizione dei documenti in differenti contenitori o shards che posson esser memorizzati su singoli o multipli nodi
* bilanciamento degli shards attraverso i nodi
* duplicazione di ogni shard per garantire dati ridondanti
* indirizzamento delle richieste da ogni nodo nel cluster ai nodi che hanno il dato interessato
* integrazione di nuovi nodi nel cluster

#### Funzionamento
La creazione di indici o tipi, il popolamento degli stessi e le query di ricerca vengono tutte effettuate tramite chiamate Http (PUT o POST per la creazione e il popolamento, GET per la ricerca). Nella creazione di un indice può esser anche specificato il mapping dei singoli campi (altrimenti ES ne deriva il tipo in automatico) e soprattutto gli **analyzers** coinvolti globalmente o sui singoli campi.  
Gli analyzers sono una specie di filtro che permette di elaborare le singole parole in input (da memorizzare o da cercare) per ricavarne il corretto valore normalizzato su cui fare l'operazione; tali analyzers servono soprattutto per uniformare le parole all'interno dei documenti per favorire la ricerca full-text. Esistono quindi analyzer per trasformare tutte le parole in minuscolo, altri che tolgono le congiunzioni o altre parole non rilevanti del linguaggio utilizzato, altri che trasformano tutte le parole al singolare o altri che prendono solo le radici delle parole (utili sia per singolare/plurale che per uniformare le forme verbali); esiste un set di analyzer già esistenti per ogni lingua ma posson anche esser creati dall'utente.  

#### Analisi

In ES posson esser fatte analisi sia di dati che di performance.  
Per il primo caso possono essere effettuate query di aggregazione basate su buckets (es. *group by*) e su metric (es. *sum* oppure *count*). Le query posson esser semplice aggregazioni oppure posson esser effettuate per generare istogrammi o grafici a linee.  
Le analisi delle performance possono esser effettuate installando Marvel ed utilizzando l'applicazione Marvel Kibana per il monitoraggio. Tramite questo strumento è possibile vedere lo stato e le performance dei cluster in real time ed analizzare gli stessi cluster, gli indici, i nodi fornendo dati su memoria occupata, shards utilizzati, tempi di risposta, etc.

### Conclusioni
- **Pro e Contro**  
**Pro** Elasticsearch è un server di ricerca molto potente ed indicato per effettuare ricerche di testo all'interno di libri, documenti, faq, etc. che mette a disposizione strumenti molto utili per l'utente finale come ordinamento dei risultati per rilevanza o i suggerimenti *forse stavi cercando* come siamo abituati a vedere nei motori di ricerca. Un altro punto a suo favore è di esser distribuibile facilmente e in maniera quasi del tutto trasparente allo sviluppatore, garantendo scalabilità dei dati senza grosse preoccupazioni.  
**Contro** Elasticsearch ha una struttura diversa rispetto ai classici db relazionali su cui siamo abituati a lavorare per cui richiede un minimo di tempo di apprendimento iniziale per capirne la struttura e come effettuarci le operazioni. Inoltre, proprio perchè le query non sono semplici come sui db che conosciamo ma contemplano anche punteggi di rilevanza, suggerimenti ed altre feature, ci vuole molto lavoro di studio su come configurare sia il mapping degli indici (decidendo e creando i giusti analyzer per filtrare meglio i termini su cui effettuare le ricerche) sia le query da effettuare (settando i giusti valori ai parametri di ricerca)
- **Possibile utilizzo in Ambrogio**
Sulla piattaforma Youneed, Elasticsearch è difficilmente utilizzabile in quanto la maggior parte delle ricerche posson esser effettuate utilizzando i DB relazionali come è già ora in quanto si tratta principalmente di ricerche non full-text. Gli unici casi in cui potrebbe aver un senso utilizzare ES sono la ricerca del testo nel wiki e la ricerca di testo nelle chat, ma ciò comporterebbe una riprogettazione dei database e del codice; inoltre, per la semplicità della funzionalità e per la media di utilizzo da parte degli utenti, gran parte del potenziale di ES sarebbe sprecato e sarebbe quasi come "sparare ad una mosca con un cannone".  
Al di fuori di Youneed potrebbe aver un senso utilizzare Elasticsearch qualora venissero sviluppate piattaforme nuove, in cui quindi non sono presenti già DB e la progettazione del salvataggio dei dati può esser fatta ex-novo, finalizzate alla raccolta di documenti/procedure aziendali oppure per l'inserimento e la ricerca di faq riguardanti programmi esistenti come Youneed.
- **Sperimentazione per Progetto Join**
