# 🌐 Stack di Rete

Questa tabella serve come guida per definire il tipo di tecnologia da utilizzare per ciascun tipo di app.
In particolare viene fatta un analisi delle migliori pratiche nella scelta della tecnologia più adatta per vari casi d'uso, in base alle necessità specifiche di performance, sicurezza, latenza, e compatibilità.

## 🧭 **Guida pratica: Stack consigliato per diversi scenari**

| **Caso d'uso**                                | **Livello 7 (Applicazione)**                         | **Livello 6 (Presentazione)** | **Livello 5-4 (Sessione/Trasporto)**     | **Commento / Obiettivo**                                                                      |
| --------------------------------------------- | ---------------------------------------------------- | ----------------------------- | ---------------------------------------- | --------------------------------------------------------------------------------------------- |
| 🎮 **FPS multiplayer**                        | 🔸 **UDP** (grezzo) o **WebSocket**                  | N/A                           | **UDP**, + STUN/TURN (per NAT traversal) | Serve latenza minima, anche a costo di perdere qualche pacchetto                              |
| 🛒 **E-commerce**                             | 🔸 **REST** o **GraphQL**                            | 🔐 **TLS**                    | **TCP**, eventualmente HTTP/2            | Serve affidabilità, integrità e compatibilità con browser e motori di ricerca                 |
| 🌐 **Sito statico**                           | 🔸 **HTTP/2 o HTTP/3 (QUIC)**                        | 🔐 TLS (facoltativo)          | **TCP o QUIC**                           | Nessuna logica dinamica, solo file HTML/CSS/JS distribuiti rapidamente                        |
| 🧠 **Sito dinamico**                          | 🔸 REST / GraphQL / WebSocket (notifiche)            | 🔐 TLS                        | **TCP**, WebSocket opzionale             | Dipende dal tipo di interazione (es. messaggi in tempo reale → WebSocket)                     |
| 📱 **IoT / Mobile Low-Power**                 | 🔸 MQTT / CoAP                                       | 🔐 TLS (opzionale)            | **UDP o TCP**                            | Protocollo minimale e binario, pensato per sensori e reti a bassa energia                     |
| 📈 **Trading app**                            | 🔸 WebSocket / gRPC                                  | 🔐 TLS                        | **TCP** (a volte QUIC)                   | Serve push continuo di dati con latenza minima e alta affidabilità                            |
| 🧾 **Banca / Finanza**                        | 🔸 SOAP (legacy), REST, gRPC (moderni)               | 🔐 TLS obbligatorio           | **TCP**                                  | Fortissima attenzione a sicurezza, conformità, affidabilità (sacrificano velocità)            |
| 🧪 **API interne tra microservizi**           | 🔸 gRPC / REST                                       | Nessuno o TLS                 | TCP o HTTP/2                             | Prediligono prestazioni interne, spesso senza crittografia esterna                            |
| 📞 **VoIP / Comunicazione**                   | 🔸 SIP, WebRTC                                       | 🔐 TLS                        | **UDP**, RTP, WebSocket                  | Deve gestire traffico in tempo reale con bassa latenza e supporto multimedia                  |
| 🏥 **Sanità / Medicale**                      | 🔸 **REST**, HL7, FHIR                               | 🔐 TLS obbligatorio           | **TCP/IP**                               | Alta priorità su sicurezza e compliance con normative come HIPAA, gestione dati sensibili     |
| 🏢 **Gestione Documentale**                   | 🔸 **REST** o **SOAP**                               | 🔐 TLS                        | **HTTP/HTTPS**                           | Necessario gestire flussi di documenti e risorse in modo affidabile e sicuro                  |
| 📣 **Notifiche Push / Marketing**             | 🔸 **WebSocket**, **Firebase Cloud Messaging (FCM)** | 🔐 TLS                        | **TCP**                                  | Notifiche in tempo reale, in particolare per messaggi promozionali e aggiornamenti            |
| 📚 **Educazione / LMS**                       | 🔸 **REST**, **GraphQL**                             | 🔐 TLS                        | **HTTP**                                 | Piattaforme educative che forniscono contenuti multimediali, discussioni in tempo reale       |
| 🎮 **Gioco multiplayer asincrono**            | 🔸 **REST**, **GraphQL**, **gRPC**                   | 🔐 TLS                        | **HTTP**, **WebSocket**                  | Gestione di giochi che non richiedono bassa latenza, ma sincronizzazione a intervalli         |
| 🧩 **PSA (Professional Services Automation)** | 🔸 **REST**, **SOAP**, **gRPC**                      | 🔐 **TLS**                    | **TCP/IP**, WebSocket                    | Gestione flussi di lavoro, ticket, risorse e servizi; alta attenzione alla gestione aziendale |
| 🔧 **RMM (Remote Monitoring and Management)** | 🔸 **REST**, **gRPC**                                | 🔐 **TLS**                    | **TCP/IP**, WebSocket                    | Monitoraggio in tempo reale dei dispositivi, operazioni remote in sicurezza                   |
| 🏭 **ERP (Enterprise Resource Planning)**     | 🔸 **REST**, **SOAP**, **gRPC**                      | 🔐 **TLS**                    | **TCP/IP**, WebSocket, Event Streams     | Pianificazione risorse aziendali, moduli separati, gestione complessa dei dati aziendali      |
| 🧑‍💼 **CRM (Customer Relationship Management)** | 🔸 **REST**, **GraphQL**, **SOAP**                   | 🔐 **TLS**                    | **TCP/IP**, HTTP/2                       | Gestione dei dati client, vendite, marketing, e relazioni con i clienti                       |
| 📄 **CMS (Content Management System)**        | 🔸 **REST**, **GraphQL**                             | 🔐 **TLS**                    | **TCP/IP**                               | Creazione e gestione contenuti, flussi dinamici, soluzioni scalabili per contenuti web        |
| 🌍 **MMO (Massively Multiplayer Online RPG)** | 🔸 **WebSocket**, **gRPC**                           | 🔐 **TLS**                    | **UDP**, **TCP/IP**, **WebSocket**       | Ambiente di gioco persistente, comunicazione continua in tempo reale, bassa latenza           |

---

### **Spiegazioni dei casi d'uso nella tabella:**

- **FPS multiplayer**: Giochi online che richiedono una bassa latenza e velocità di comunicazione. Utilizzano **UDP** per garantire la velocità e **WebSocket** per la comunicazione bidirezionale in tempo reale. **STUN/TURN** vengono utilizzati per il NAT traversal, consentendo ai giocatori di connettersi attraverso reti con firewall.

- **E-commerce**: Piattaforme di vendita online che necessitano di un'interfaccia utente affidabile, supporto per transazioni sicure e integrazione con motori di ricerca e browser. Si utilizza **REST** o **GraphQL** per la comunicazione con il backend e **TLS** per la sicurezza, con **TCP** per garantire la stabilità della connessione.

- **Sito statico**: Pagine web con contenuti fissi che vengono serviti direttamente dai server senza interazione dinamica. Si utilizzano **HTTP/2** o **HTTP/3 (QUIC)** per ottimizzare i tempi di caricamento, e **TLS** può essere facoltativo. L'uso di **TCP** o **QUIC** migliora la velocità e l'affidabilità della connessione.

- **Sito dinamico**: Siti web che richiedono un'interazione continua con il backend, come l'aggiornamento dei contenuti in tempo reale o l'interazione con gli utenti. Si usa **REST** o **GraphQL** per la gestione delle risorse e delle richieste. **WebSocket** è utile per la comunicazione bidirezionale in tempo reale, mentre **TLS** assicura la sicurezza delle informazioni trasmesse.

- **IoT / Mobile Low-Power**: Sistemi che utilizzano dispositivi a bassa potenza, come sensori o dispositivi mobili. I protocolli **MQTT** e **CoAP** sono progettati per ottimizzare la trasmissione di piccoli pacchetti di dati su reti a bassa energia. **TLS** è opzionale ma raccomandato per la sicurezza, mentre **UDP** o **TCP** sono utilizzati per la comunicazione.

- **Trading app**: Applicazioni finanziarie che richiedono aggiornamenti in tempo reale e alta affidabilità dei dati. **WebSocket** o **gRPC** sono utilizzati per la comunicazione bidirezionale in tempo reale, e **TLS** garantisce la sicurezza. **TCP** è utilizzato per la stabilità della connessione, mentre **QUIC** può essere impiegato per migliorare le prestazioni.

- **Banca / Finanza**: Settore che richiede un elevato livello di sicurezza e affidabilità per gestire dati sensibili, transazioni e conformità alle normative. Si usano **SOAP** (per soluzioni legacy) o **REST**/**gRPC** per la comunicazione, con **TLS** obbligatorio per proteggere i dati.

- **API interne tra microservizi**: Comunicazione tra moduli separati di un'applicazione, dove la prestazione e l'affidabilità sono essenziali. Si utilizza **gRPC** o **REST** per la comunicazione, senza la necessità di crittografia esterna (se non necessario), e si privilegia **TCP** o **HTTP/2** per prestazioni elevate.

- **VoIP / Comunicazione**: Sistemi per la trasmissione di voce e video in tempo reale. Si usano protocolli come **SIP**, **WebRTC**, **RTP** e **WebSocket** per la trasmissione dei dati, con **TLS** per garantire la sicurezza della comunicazione.

- **Sanità / Medicale**: Piattaforme che gestiscono dati sensibili dei pazienti e operano in ambienti ad alta sicurezza e privacy. Si usano **REST** o **FHIR** per l'interoperabilità tra sistemi, con **TLS** obbligatorio per proteggere i dati. È essenziale rispettare le normative come HIPAA.

- **Gestione Documentale**: Sistemi per la gestione di documenti e risorse aziendali. Si utilizza **REST** o **SOAP** per l'accesso ai dati e **HTTPS** per garantire la sicurezza nelle comunicazioni.

- **Notifiche Push / Marketing**: Piattaforme che inviano notifiche in tempo reale agli utenti. Utilizzano **WebSocket**, **Firebase Cloud Messaging (FCM)**, o altre tecnologie di push notification per garantire comunicazioni rapide e sicure. **TLS** è utilizzato per proteggere le comunicazioni.

- **Educazione / LMS**: Sistemi di gestione dell'apprendimento che richiedono interazioni in tempo reale, video, e contenuti multimediali. Si utilizza **REST** o **GraphQL** per il backend e **TLS** per garantire la sicurezza dei dati. Il supporto per interazioni in tempo reale (chat, discussioni) può essere gestito tramite **WebSocket**.

- **Gioco multiplayer asincrono**: Giochi che non richiedono sincronizzazione in tempo reale ma necessitano comunque di comunicazioni tra giocatori. Si usa **REST**, **GraphQL** o **gRPC** per la gestione delle richieste e dei dati, mentre **WebSocket** può essere utilizzato per notifiche asincrone.

- **PSA (Professional Services Automation)**: Sistemi utilizzati per l'automazione di servizi professionali, come ticketing, gestione risorse e flussi di lavoro aziendali. Utilizzano **REST**, **SOAP**, o **gRPC** per la gestione delle operazioni, e **WebSocket** per aggiornamenti in tempo reale.

- **RMM (Remote Monitoring and Management)**: Piattaforme per monitorare e gestire dispositivi a distanza. Si utilizzano **REST** o **gRPC** per la comunicazione, con **TLS** per la sicurezza, e **WebSocket** per il monitoraggio in tempo reale dei dispositivi.

- **ERP (Enterprise Resource Planning)**: Sistemi complessi che gestiscono risorse aziendali come contabilità, produzione, logistica, e risorse umane. Si utilizzano **REST**, **SOAP**, o **gRPC** per la comunicazione, con **TLS** obbligatorio per la protezione dei dati sensibili. La gestione dei dati interni avviene tramite **TCP/IP** e **WebSocket** per aggiornamenti in tempo reale.

- **CRM (Customer Relationship Management)**: Sistemi per la gestione delle relazioni con i clienti, dalle vendite al supporto. Utilizzano **REST**, **GraphQL** o **SOAP** per la gestione delle informazioni, con **TLS** per garantire la sicurezza.

- **CMS (Content Management System)**: Sistemi per la gestione dei contenuti web, come WordPress o Joomla. Si utilizzano **REST** o **GraphQL** per l'interazione tra frontend e backend, con **TLS** per la sicurezza delle comunicazioni.

- **MMO (Massively Multiplayer Online RPG)**: Giochi online persistenti che supportano milioni di utenti. Richiedono una comunicazione continua in tempo reale, con **WebSocket** per la sincronizzazione e **UDP** o **TCP/IP** per ridurre la latenza e migliorare la velocità. **TLS** è utilizzato per proteggere i dati degli utenti.

---

## 📌 **Come scegliere in pratica?**

1. **Real-time / Comunicazione in tempo reale > UDP / WebSocket / gRPC**

   - Adatto per giochi multiplayer, trading, applicazioni di messaggistica, e qualsiasi sistema che richiede interazioni in tempo reale o bassa latenza.

2. **Compatibilità Browser / Siti web generali > REST / GraphQL**

   - La soluzione ideale per applicazioni che devono essere compatibili con i browser moderni e che necessitano di una gestione efficiente delle risorse, come gli e-commerce o i siti dinamici.

3. **Performance Inter-servizio > gRPC / HTTP/2**

   - Ottimo per comunicazioni tra microservizi o sistemi complessi dove le prestazioni sono fondamentali, come negli ERP o nelle API interne.

4. **Siti Vetrina (Statici) > HTTP/2 o HTTP/3 (QUIC)**

   - Utilizzato per ottimizzare i tempi di caricamento dei siti web statici, migliorando l’esperienza utente e la visibilità sui motori di ricerca.

5. **Interfacce Lente / Banda Limitata (IoT) > MQTT / CoAP**

   - Adatto per dispositivi IoT o applicazioni mobili che operano su reti con larghezza di banda limitata, dove la velocità e l’efficienza sono cruciali.

6. **Sicurezza Elevata > TLS (SSL)**

   - Indispensabile per sistemi che gestiscono dati sensibili, come le applicazioni bancarie, finanziarie, CRM e ERP, dove la protezione dei dati è una priorità.

7. **Affidabilità e Conformità > SOAP**

   - Utilizzato in ambienti aziendali complessi e legacy dove è necessaria un'interoperabilità con sistemi più vecchi e la compliance con normative rigorose, come in finanza e sanità.

8. **Aggiornamenti Push in Tempo Reale > WebSocket / Firebase Cloud Messaging**

   - Adatto per notifiche push, messaggistica istantanea, trading app, o piattaforme che devono inviare aggiornamenti costanti agli utenti.

9. **Scalabilità e Moduli Separati > Microservizi / gRPC**

   - Ideale per sistemi aziendali complessi come ERP o CRM, dove diversi moduli (come contabilità, produzione, HR) devono essere separati ma interagire tra loro in modo efficiente.

10. **Connessioni a Bassa Latenza > UDP / WebSocket**

    - Per applicazioni che richiedono connessioni con latenza minima, come giochi FPS, applicazioni di trading in tempo reale, o sistemi di comunicazione in tempo reale.

11. **Gestione di Risorse in Tempo Reale > gRPC / WebSocket**

    - Adatto a piattaforme come RMM, PSA e sistemi di monitoraggio che necessitano di interazioni in tempo reale per la gestione delle risorse.

12. **API Legacy o Interoperabilità > SOAP / REST**

    - Utilizzato per applicazioni che necessitano di comunicare con sistemi legacy o devono mantenere una compatibilità con applicazioni esistenti, come nel caso di piattaforme aziendali legacy.

13. **Gestione Documenti / Contenuti > REST / GraphQL**

    - Adatto per CMS, gestione documentale e sistemi simili, dove l’interazione con i contenuti e la facilità d’uso sono essenziali.

14. **Esperienza utente con messaggi e notifiche > WebSocket**

    - Perfetto per giochi, piattaforme di messaggistica o qualsiasi sistema che richiede la trasmissione di messaggi o notifiche in tempo reale.

15. **Rete Mobile o Bassa Potenza > MQTT / CoAP**
    - Utilizzato per dispositivi con consumi energetici ridotti, come sensori IoT o applicazioni mobili che operano su reti a bassa potenza o larghezza di banda limitata.
