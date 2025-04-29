Una **tabella esaustiva** dei principali **sistemi di accesso/autenticazione** 📋:

| **Metodo di Accesso** | **Descrizione** | **Tipico uso** | **Protocollo/Formato** |
|:----------------------|:----------------|:---------------|:-----------------------|
| **SAML** | Scambio XML firmato di informazioni di autenticazione/autorizzazione | Enterprise SSO, Intranet aziendali | XML |
| **OAuth 2.0** | Delegazione di accesso tra applicazioni | App mobile/web per accesso a risorse di terzi | Token (Bearer Token, JWT) |
| **OpenID Connect (OIDC)** | Estensione di OAuth2 per autenticazione (non solo autorizzazione) | Login su app web/mobile (es: "Accedi con Google") | JWT |
| **LDAP (Lightweight Directory Access Protocol)** | Accesso centralizzato ad account e directory di utenti | Autenticazione interna in reti aziendali | Protocollo binario/text |
| **Kerberos** | Sistema di autenticazione basato su ticket | Ambienti Windows, reti aziendali | Ticket criptati |
| **RADIUS (Remote Authentication Dial-In User Service)** | Autenticazione remota di utenti, spesso usato su VPN/Wi-Fi aziendali | VPN, hotspot Wi-Fi aziendali | UDP |
| **TACACS+** | Simile a RADIUS ma più orientato al controllo di accesso su dispositivi di rete | Accesso ad apparati di rete (router, switch) | TCP |
| **WS-Federation** | Simile a SAML, scambio di informazioni di identità tramite SOAP | Legacy Microsoft SSO | XML/SOAP |
| **X.509 Certificate Authentication** | Login tramite certificati digitali | Accesso sicuro a server e VPN | Certificato X.509 |
| **Password tradizionali** | Username + password locali | Tutte le app senza SSO | Testuale |
| **Biometria (Face ID, impronta)** | Autenticazione tramite caratteristiche fisiche | Mobile, dispositivi personali | Biometrics API |
| **Magic Link** | Autenticazione senza password tramite link inviato via email | App moderne senza password | URL token |
| **Social Login** | Autenticazione tramite social network (es: Facebook, Google, Apple) | App mobile/web | OAuth2 / OpenID Connect |
| **FIDO2 / WebAuthn** | Standard per autenticazione passwordless con dispositivi sicuri (es: YubiKey, FaceID) | Siti web moderni, aziende | Public Key Cryptography |

---

# 📌 Commenti veloci:
- **SAML** = super usato nei grandi sistemi aziendali.
- **OAuth2 / OpenID Connect** = domina nel web e mobile moderno.
- **Kerberos** = forte nelle reti interne (specialmente Windows).
- **FIDO2 / WebAuthn** = futuro senza password.
- **Magic Link** = nuova tendenza per autenticazioni rapide senza ricordare password.

---
