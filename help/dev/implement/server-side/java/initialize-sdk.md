---
title: Inizializzare Java SDK utilizzando il metodo create
description: Scopri come utilizzare il metodo create per inizializzare Java SDK e creare un’istanza di [!UICONTROL TargetClient] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.
feature: APIs/SDKs
exl-id: 0e0ddead-7de8-4549-b81c-e72598558e4b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 18%

---

# Inizializzare l’SDK Java

## Descrizione

Utilizza il `create` per inizializzare l&#39;SDK Java e creare un&#39;istanza del [!UICONTROL Client di destinazione] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.

## Metodo

[!UICONTROL TargetClient] viene creato utilizzando `TargetClient.create`.

### creare

```javascript {line-numbers="true"}
TargetClient TargetClient.create(ClientConfig clientConfig)
```

ClientConfig viene creato utilizzando `ClientConfig.builder`.

```javascript {line-numbers="true"}
ClientConfigBuilder ClientConfig.builder()
```

## Parametri

`ClientConfigBuilder` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | impostazione predefinita | Descrizione |
| --- | --- | --- | --- | --- |
| client | Stringa | Sì | None (Nessuno) | [!UICONTROL ID client di destinazione] |
| organizationId | Stringa | Sì | None (Nessuno) | [!UICONTROL ID organizzazione Experience Cloud] |
| connectTimeout | Numero | No | 10000 | Timeout della connessione per tutte le richieste in millisecondi |
| socketTimeout | Numero | No | 10000 | Timeout socket per tutte le richieste in millisecondi |
| maxConnectionsPerHost | Numero | No | 100 | Numero massimo di connessioni per [!DNL Target] host |
| maxConnectionsTotal | Numero | No | 200 | Numero massimo di connessioni, inclusi tutti [!DNL Target] host |
| enableRetries | Booleano | No | true | Tentativi automatici per timeout socket (massimo 4) |
| logRequests | Booleano | No | false | Log [!DNL Target] richieste e risposte nel debug |
| logRequestStatus | Booleano | No | false | Log [!DNL Target] tempo di risposta, stato e URL |
| serverDomain | Stringa | No | `*client*.tt.omtrdc.net` | Sostituisce il nome host predefinito |
| protetto | Booleano | No | true | Annulla l&#39;impostazione per applicare lo schema HTTP |
| requestInterceptor | HttpRequestInterceptor | No | Null | Aggiungi intercettore di richiesta personalizzato |
| defaultPropertyToken | Stringa | No | None (Nessuno) | Imposta il token di proprietà predefinito per ogni `getOffers` chiamare. **Per il decisioning sul dispositivo**, l&#39;SDK scaricherà solo l&#39;artefatto che contiene le attività qualificate per il token di proprietà impostato in `defaultPropertyToken` |
| defaultDecisioningMethod | Enum DecisioningMethod | No | LATO_SERVER | Deve essere impostato su ON_DEVICE o HYBRID per abilitare le decisioni sul dispositivo |
| telemetryEnabled | Booleano | No | true | Consente ai clienti di rinunciare alla raccolta di dati aggiuntivi durante le richieste a [!DNL Target] server |
| proxyConfig | ClientProxyConfig | No | None (Nessuno) | Consente al cliente di fornire i propri dettagli proxy |
| exceptionHandler | TargetExceptionHandler | No | None (Nessuno) | Può essere utilizzato per implementare la gestione delle eccezioni personalizzate durante l&#39;elaborazione delle regole |
| httpClient | HttpClient | No | None (Nessuno) | Consente agli utenti di sostituire il [!DNL Target] Client HTTP con un client HTTP personalizzato |
| onDeviceEnvironment | Stringa | No | produzione | Può essere utilizzato per specificare un ambiente su dispositivo diverso, ad esempio staging |
| onDeviceConfigHostname | Stringa | No | `assets.adobetarget.com` | Può essere utilizzato per specificare un host diverso da utilizzare per scaricare il file dell’artefatto di decisioning sul dispositivo |
| onDeviceDecisioningPollingIntSecs | int | No | 300 (5 minuti) | Numero di secondi tra i recuperi del file dell’artefatto di decisioning sul dispositivo |
| onDeviceArtifactPayload | byte[] | No | None (Nessuno) | Fornisce decisioni sul dispositivo con precedente payload dell’artefatto per consentire l’esecuzione immediata |
| onDeviceDecisioningHandler | OnDeviceDecisioningHandler | No | None (Nessuno) | Registra i callback per gli eventi di decisioning sul dispositivo |
| onDeviceAllMatchingRulesMboxes | Elenco\&lt;string> | No | None (Nessuno) | Consente agli utenti di specificare le mbox per le quali verrà restituito tutto il contenuto della regola corrispondente durante il decisioning sul dispositivo |

## Esempio

### Java

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient.create(clientConfig);

// make calls to Adobe Target
```
