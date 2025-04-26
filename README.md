# Reti-Protocolli

Il modello OSI è fondamentale per comprendere e progettare reti di comunicazione, offrendo una struttura modulare che facilita l'interoperabilità tra diversi sistemi e protocolli. Sebbene nella pratica molte reti utilizzino il modello TCP/IP, che combina alcuni di questi livelli, il modello OSI rimane una guida teorica essenziale per l'analisi e la risoluzione dei problemi di rete .

### Alcuni link utili:

- ([Modello OSI - DataSunrise](https://www.datasunrise.com/it/centro-di-conoscenza/modello-osi/))

- ([Livello di applicazione: il livello 7 del modello ISO/OSI - IONOS](https://www.ionos.it/digitalguide/server/know-how/livello-di-applicazione/))

- ([Cos'è il Modello OSI? - Spiegazione dei 7 livelli OSI - AWS](https://aws.amazon.com/it/what-is/osi-model/))

---

### 📶 Livello 1 – **Fisico** (Physical Layer)

- **Funzione**: Trasmette i bit grezzi attraverso un mezzo fisico, come cavi o onde radio.
- **Esempi**: Ethernet, USB, Bluetooth, Wi-Fi, cavi in fibra ottica.

---

### 🔗 Livello 2 – **Collegamento Dati** (Data Link Layer)

- **Funzione**: Assicura una trasmissione dati priva di errori tra due nodi adiacenti, gestendo l'indirizzamento fisico e il controllo degli errori.
- **Esempi**: Ethernet, PPP, MAC (Media Access Control), LLC (Logical Link Control).

---

### 🌐 Livello 3 – **Rete** (Network Layer)

- **Funzione**: Gestisce l'instradamento dei pacchetti tra dispositivi su reti diverse, determinando il percorso ottimale.
- **Esempi**: IP (IPv4, IPv6), ICMP, IPsec.

---

### 🚚 Livello 4 – **Trasporto** (Transport Layer)

- **Funzione**: Fornisce una comunicazione end-to-end affidabile, gestendo il controllo del flusso e la correzione degli errori.
- **Esempi**: TCP (Transmission Control Protocol), UDP (User Datagram Protocol).

---

### 🧩 Livello 5 – **Sessione** (Session Layer)

- **Funzione**: Stabilisce, gestisce e termina le sessioni tra applicazioni in comunicazione.
- **Esempi**: NFS (Network File System), SMB (Server Message Block), NetBIOS.

---

### 🖼️ Livello 6 – **Presentazione** (Presentation Layer)

- **Funzione**: Traduce i dati tra il formato dell'applicazione e il formato di rete, gestendo la crittografia e la compressione.
- **Esempi**: SSL/TLS, JPEG, MPEG, ASCII, EBCDIC.

---

### 🧑‍💻 Livello 7 – **Applicazione** (Application Layer)

- **Funzione**: Fornisce servizi di rete direttamente alle applicazioni dell'utente finale.
- **Esempi**: HTTP, FTP, SMTP, DNS, SNMP.

---

## 🧱 **Peso e Efficienza nei Livelli OSI**

### ✏️ I principali motivi per cui un pacchetto diventa più grande:

- Ogni livello **aggiunge header/metadata** (es. IP header, TCP header, TLS handshake, ecc.)
- Alcuni protocolli fanno **compressione** o **crittografia**, che possono ridurre o aumentare il peso
- Alcuni protocolli sono **verbose** (parlano tanto!) mentre altri sono **compatti** (stringati)

---

## 📊 **Comparativa: Peso e Efficienza**

| **Livello**       | **Protocollo / Tecnologia** | **Peso**                  | **Commento**                                                |
| ----------------- | --------------------------- | ------------------------- | ----------------------------------------------------------- |
| 7 - Applicazione  | REST (HTTP+JSON)            | 🚚 Pesante                | Verboso, tanti caratteri, JSON non compresso                |
| 7 - Applicazione  | gRPC (HTTP/2 + Protobuf)    | ✈️ Leggero                | Super compatto grazie a Protobuf (binario)                  |
| 7 - Applicazione  | GraphQL                     | 🚚 Medio-Pesante          | Più snello di REST, ma comunque testuale                    |
| 7 - Applicazione  | SOAP (XML)                  | 🐘 Molto pesante          | XML molto prolisso                                          |
| 6 - Presentazione | TLS/SSL                     | 📈 Più peso               | Aggiunge handshake iniziale + crittografia                  |
| 5 - Sessione      | WebSocket                   | ✈️ Leggero dopo handshake | Inizialmente pesante (handshake HTTP), poi messaggi piccoli |
| 4 - Trasporto     | TCP                         | 🚚 Pesante                | Affidabile, ma porta overhead di gestione pacchetti         |
| 4 - Trasporto     | UDP                         | 🪶 Ultra leggero          | Non affidabile ma velocissimo e senza overhead              |
| 3 - Rete          | IP (IPv4)                   | Normale                   | Header di 20 byte circa                                     |
| 2 - Data Link     | Ethernet                    | Normale                   | Aggiunge poco, ma fisicamente necessario                    |
| 1 - Fisico        | Fibra, Wi-Fi, Ethernet cavi | --                        | Solo i bit fisici, nessun peso "di protocollo"              |

---

## 🎯 **Sintesi veloce**

- **Protocolli leggeri**: gRPC, UDP, WebSocket (dopo apertura)
- **Protocolli pesanti**: REST, SOAP, TCP, TLS (in apertura)
- **Protocolli medi**: GraphQL, IP, Ethernet

---

## 📈 Esempio pratico

Se mandi **un messaggio "Ciao"** con:

- **REST + HTTP/1.1 + TCP** = 800–1000 byte (testata enorme + JSON + TCP/IP overhead)
- **gRPC su HTTP/2 + TCP** = 100–200 byte (Protobuf binario + header compatti)
- **WebSocket su TCP** (dopo handshake) = 30–50 byte (molto rapido)
- **UDP diretto** = 20–40 byte (ma rischi perdita dati!)

---

## 📚 Conclusione

- Se vuoi **efficienza pura**: preferisci **gRPC**, **WebSocket** o **UDP** (se puoi sacrificare affidabilità).
- Se ti serve **compatibilità universale**: REST vince, anche se è più pesante.
- Se ti serve **messaggistica compatta IoT / Mobile**: MQTT è pazzescamente leggero.

---

### - Se stai creando un'applicazione consulta la tabella dello [Stack Consigliato](Stack-Consigliato.md) per applicare la configurazione migliore.

### - Puoi visualizzare anche una [Matrice Decisionale](Matrice-Decisionale.md) per capire come impostare ciascun tipo di applicazione.

---
