# I Token come sistema di sicurezza

[Link al Tutorial](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)

Le app moderne sfruttano un sistema di sicurezza basato sull'utilizzo dei Token.
I token usati sono di 3 tipi:

1. ID Token
2. Refresh Token
3. Access Token

Un token è una porzione di dati con informazioni sufficenti ad effettuare operazioni:

1. che determinano l'identità utente
2. che autorizzano un utente ad eseguire un'azione

I Token sono degli artefatti che consentono alle applicazioni di eseguire processi di autorizzazione e autenticazione.
Alcuni dei frameworks più tilizzati per questo scopo sfruttano strategie basate sui tokens per proteggere l'accesso ad app e risorse.

Un esempio di utilizzo strategico è:

- **Autorizzazione** → tramite OAuth 2.0
- **Autenticazione** → tramite OpenID Connect

---

### OAuth 2.0

Di base OAuth viene utilizzato per accedere a risorse localizzate su un altro server, per conto dell'utente.

La versione OAuth 2.0 utilizza:

- **Token di Accesso**
- **Token di aggiornamento**

---

### OpenID Connect

OpenID Connect consente all'utente di :

1. autenticare l'utente
2. chiedere il consenso all'utente
3. emettere i Token

OpenID Connect utilizza:

- **ID Token**

---

### I Token ID

Un **ID Token** è un artefatto che le applicazioni client possono usare per consumare l'identità di un utente.

Un Token ID può contenere informazioni come:

1. nome
2. email
3. immagine utente

Le applicazioni possono quindi prendere queste informazioni e itilizzarle per creare un profilo e personalizzare l'esperienza di quell'utente.

Il server di autenticazione che usa **OpenID Connect** per implementare l'autenticazione, emette un **Token ID** per i suoi client quando l'utente esegue l'accesso.

I consumers di Token ID sono principalmente SPA(Single-Page-Apps) e Applicazioni Mobile.

---

### Access Tokens

Un **Access Token** è un token emesso dal server di autorizzazione nel momento in cui un utente esegue l'accesso.
Questo "Token di Accesso" può essere usato per effettuare chiamate API sicure, nel momento in cui è necessario proteggere delle risorse per conto dell'utente.

L'Access Token segnala al server che il client è stato autorizzato dall'utente per eseguire alcuni compiti, o accedere a certe risorse.

L' _OAuth 2.0_ non definisce il formato di un **Access Token** ma lo emette seguendo uno standard JWT(Json Web Token) per i Token delle API.

Lo schema sotto riassume le parti che compongono un JWT (Struttura di esempio)

```json
HEADER:    {"alg": "HS256", "typ": "JWT"}
    ↓ Base64URL
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9

PAYLOAD:   {"sub": "1234567890", "name": "Mario Rossi", "iat": 1516239022}
    ↓ Base64URL
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6Ik1hcmlvIFJvc3NpIiwiaWF0IjoxNTE2MjM5MDIyfQ

SIGNATURE:  HMACSHA256( base64UrlEncode(header) + "." + base64UrlEncode(payload), secret )
```

Si può notare che è formato da 3 parti:

- un header
- un payload
- una signature

**IMPORTANTE :**
Un Access Token è un token di trasporto dati, il che significa che qualsiasi malintenzionato potrebbe entrarne in possesso (vuilando il sistema) ed utilizzarlo per rubare accessi o dati riservati, o addirittura violare conti privati.
Per prevenire questo furto di dati è necessario adottare le migliori misure di sicurezza che riducono al minimo il rischio di compromettere gli **Access Token**.

---

### Prevenire il furto dei Token di accesso

Un modo per mettere in sicurezza i dati contenuti negli **Access Token** è quello di limitarne la durata ad un breve ciclo di vita che va da

- alcune ore
- alcuni giorni

---

### Ottenere nuovi Access Token

Esistono diversi modi in cui un client piò ottenere un nuovo **Access Token** quando scade una sessione.

1. **Rieseguire l'accesso**
   Un app client potrebbe chiedere nuovamente all'utente di eseguire l'accesso tramite credenziali per ottenere un nuovo Access Token.

2. **Tramite Refresh Token**
   Un **Authorization Server** può emettere un **Refresh Token** (token di aggiornamento) all'app client che consente di sostituire il vecchio **Access Token** ormai scaduto.

---

### Refresh Token

E' possibile migliorare la sicurezza dell'app rendendo temporanea la durata di un **Access Token**.

Quando l'**Access Token** scade le app possono usare un **Refresh Token** per ottenere un nuovo **Access Token** senza il bisogno di rieseguire il login.

L'operazione tramite **Refresh Token** è possibile fintanto che questo sia ancora _valido_ e _non scaduto_.

ATTENZIONE: Un **Refresh Token** con una vita di lunga durata potrebbe dare troppo potere di accesso alle risorse a chi entra in possesso del token.
Questo potrebbe essere sia un utente normale che un malintenzionato.

Per mitigare questo rischio OAuth ha creato un meccanismo per garantire che questo token sia utilizzato solamente dalle parti previste.

- Fin qui tutto bello, ma quando utilizzare il **Refresh Token**?
  Nel caso in cui un sistema sfrutti l'autenticazione SAML ([Vedi tabella di riepilogo](Autenticazioni-Sicure.md)), il refresh token non viene utilizzato.
  Questo può capitare in situazioni dove vengono sviluppate applicazioni enterprise, ma nell'uso più comune delle web & mobile app il sistema OAuth + OpenID è molto comune.

Ogni protocollo OAuth e OpenID rispetta una pipeline di esecuzione composta da molti vantaggi e svantaggi che servono a definire quando utilizzare uno piuttosto che un altro.

---

### Flusso di Autorizzazione + Autenticazione

Ogni flow presente nel protocollo OAuth 2.0 è ottimizzato per ciascun caso d'uso.

In **OAuth 2.0**, i **flow** (chiamati anche **grant types**) sono i modi in cui un'app ottiene un **access token**.  
Ecco **i principali flow** usati:

| **Flow**                                         | **Descrizione**                                                                                              | **Quando usarlo**                                                                         |
| :----------------------------------------------- | :----------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------- |
| **Authorization Code Flow**                      | Il client ottiene un **authorization code** che scambia con un **access token** in un secondo momento.       | Traditional web app running on server-side (backend sicuro)                               |
| **Authorization Code Flow con PKCE**             | Come sopra, ma con una protezione extra (**PKCE**) contro attacchi di interception.                          | App mobile, SPA (Single Page App), senza backend sicuro                                   |
| **Client Credentials Flow**                      | Il client autentica se stesso senza l'interazione dell'utente e ottiene l'access token.                      | Comunicazione server-to-server o Client proprietari(machine-to-machine)                   |
| **Implicit Flow**                                | Il client riceve direttamente l'access token nella risposta, senza bisogno di scambiarlo (no refresh token). | (Deprecato!) SPA tradizionali senza backend sicuro. Oggi si preferisce PKCE.              |
| **Resource Owner Password Credentials Flow**     | L'utente fornisce username e password direttamente al client per ottenere l'access token.                    | Solo in ambienti **totalmente fidati**, oggi sconsigliato! (chi è totalmente fidato ?!?!) |
| **Device Authorization Flow** (Device Code Flow) | L'utente autentica su un altro dispositivo (es: smart TV) inserendo un codice.                               | Dispositivi senza tastiera (TV, console, IoT)                                             |

---

### 📌 In breve:

- **Web App** → Authorization Code Flow
- **Mobile App / SPA** → Authorization Code Flow **con PKCE**
- **Server 2 Server** → Client Credentials Flow
- **Smart TV** → Device Authorization Flow
- **Legacy (sconsigliati)** → Implicit Flow, Resource Owner Password

---

# Sicurezza + Convenienza

Nella lista precedente si è analizzato il processo di scambio delle chiavi nelle SPA.

Questo processo è molto spesso trascurato in ambito di sicurezza poiché vengono tralasciati dei passaggi importanti come :

- la mancanza di Refresh Token
- o la pesantezza in fase si autenticazione dovuta dalla pesantezza del PKCE.

Un compromesso è possibile grazie al **Token Refresh Rotation**

---

### Refresh Token Rotation

Questa procedura di rotazione dei **Refresh Token** può rendere fruibile l'utilizzo di questi anche nelle SPA.

> Nelle specifiche di sicurezza viene precisato che se non si sa se il **Refresh Token** appartiene al client, dovremmo evitare di utilizzarlo a meno che non sia in atto il **Refresh Token Rotation**.

---

### Mantenere sicuri i Refresh Token

Un **Access Token** di breve durata aumenta la sicurezza ma peggiora l'esperienza, poiché un utente deve ogni volta riloggarsi per ottenere un nuovo **Access Token**.

I **Refresh Token** possono dare il giusto equilibrio tra sicurezza e convenienza dove : pur avendo **Access Token** scaduti possono ottenerne di nuovi senza compromettere la UX.

I **Refresh Token** sono token portatori che devono essere messi al sicuro per evitare che trapelino informazioni.

Questi Token vengono rilasciati dal server che non ha informazioni riguardo il fruente, non sa quindi chi è o meno legittimato ad ottenere questi token.
E' necessario quindi trattare tutti gli utenti come possibili malintenzionati.

Per mettere al sicuro il sistema di blocco del riutilizzo il server crea una sorta di **Family Token** che associa a ciascun token un gruppo di generazione.
Ogni nuovo refresh token avrà la stessa famiglia seppur diverso nel codice.

- Nel caso specifico di una contraffazione, un malintenzionato che riesce ad impossessarsi di un refresh token e provasse a riutilizzarlo, questo verrebbe respinto dal server.
- Se così avviene, in automatico TUTTI i Token Generati dal server che appartengono allo stesso tipo di famiglia verrebbero fatti scadere.
- Questo ssignifica che il refresh token (non ancora utilizzato) dall'utente, sarà auto-annullato poiché la famiglia di appartenenza non è più valida.
- Di conseguenza servirà ri-effettuare una nuova autenticazione che rilascerà un nuovo **Access Token** per riottenere un nuovo **Refresh Token** ed una nuova **Family Token** ad esso associata.

Il rilevamento automatico del riutilizzo del **Refresh Token** è una compoente chiave di una strategia di **Rotazione del Refresh Token**.

---

Come funzionano i **Refresh Token Rotation** ?
I RTR garantiscono che ad ogni richiesta di un **Access Token** tramite **Refresh Token**, l'applicazione riceverà un nuovo **Refresh Token**.

In questo modo i vecchi **Refresh Token** si trasformano in token monouso e quindi vengono invalidati al rilascio di ogni nuovo **Refresh Token**, di conseguenza se qualche malintenzionato riesce ad entrarne in possesso questo non potrebbe farci nulla perché il vecchio **Refresh Token** non verrà accettato come valido.

- Ad ogni richiesta tramite Refresh Token il server ne riemetterà uno nuovo, facendo scadere il precedente, il vecchio refresh token (anche se intercettato non sarà + valido).
- Ogni nuovo **Refresh Token** rilasciato dal server ha una propria **Token Family**, se viene captato un tentativo di riutilizzo TUTTA la **Token Family** viene annullata.
- Per ricevere quindi un nuovo **Refresh Token** dal server bisognerà ri-effettuare un nuovo accesso tramite credenziali che rilascerà una nuova coppia di Token (Authorization + Refresh).

> Si riduce in questo modo il rischio di attacchi di riproduzione da token compromessi.

---

### Privacy dei dati

La privacy in tutto ciò occupa un posto importante.
Quando si ha a che fare con un'app che sfrutta il **Refresh Token Rotation**, questo permette di memorizzare il **Refresh Token** su:

- _Memoria locale_
- _Memoria del Browser_

**_Memoria del Browser_**:
La memoria del browser ha il previlegio di mantenere attiva la sessione tra un refresh di percorso o una riapertura dell'app su una nuova scheda.

Alcuni sviluppi recenti di una tecnologia chiamata **ITP (Intelligent Tracking Protection)** impedisce però che la memoria del browser venga utilizzata per attività sospette e/o archiviazione di lunga durata.

Questa tecnologia, inizialmente introdotta da Safari (browser della Apple), è incentrata sulla massima protezione riguardo la tracciabilità dei dati memorizzati.
Quindi la mancanza di persistenza migliora la sicurezza del token ma tende a cancellarli e per questo motivo (se hanno una lunga durata) non sono adatti per le SPA.

**_Memoria locale_**:
Dall'altro canto il salvataggio su una memoria locale come **HDD** o **SSD** del **Sistema Operativo** potrebbe risolvere questa mancanza di persistenza del **Refresh Token**.
Ma al contrario di prima se la persistenza viene risolta il problema risiede nella sicurezza, un attaccante potrebbe iniettare tramite browser del codice JavaScript malevolo e ricevere i token archiviati su disco.

Questa vulnerabilità può portare ad un attacco CrossScripting molto probabile in un **SPA** o in qualsiasi altro sito che sfrutta **Bootstrap** o **Google Analitics**.

Quindi si cerca di ridurre il tempo di durata assoluta (tempo effettivo) del token per evitare la violazione del Token salvato su disco locale.

**Si riduce così il rischio di un attacco di tipo cross-site scripting riflesso CSSR, ma senza token persistente**.

Una lunga durata di vita di un Refresh Token potrebbe portare ad una violazione del token ma essendo influenzato dalla rotazione dei refresh token questi saranno sempre ricreati ad ogni nuova sessione (che in media è di breve durata).

---

### Token Best Practise

OAuth delinea una serie di pratiche da utilizzare per sfruttare al meglio l'efficenza dei token aggiungendo però anche sicurezza e privacy.

Queste sono alcune delle regole da seguire:

- Tienili **segreti** e al **sicuro**
- Non aggiungere mai dati sensibili ai payload
- Dai ai token una scadenza (anche di durata lunga se indispensabile ma mai infinita).
- Utilizza i protocolli HTTPS (HTTP + TSL).
- Considera (facendo uno schema) tutti i casi d'uso dei tipi di autorizzazione che servono.
- Archivia e riutilizza

Una spiegazione esaustiva delle migliori linee guida è disponibile al [link](https://auth0.com/docs/secure/tokens/token-best-practices)

La **dashboard** di _OAuth_ è studiata per semplificare la configurazione dei servizi di **autorizzazione** ed **autenticazione** dei _Refresh Token_.

Gli SDK e le librerie OAuth sono studiate per supportare i _Refresh Token_ per:

- Web App
- Mobile App
- Single Page Apps

Altre risorse su come utilizzare i refresh token sono disponibili al blog linkato anche [sopra](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)

---
