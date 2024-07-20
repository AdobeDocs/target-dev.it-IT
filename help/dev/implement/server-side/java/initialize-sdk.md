---
title: Inizializzare Java SDK utilizzando il metodo create
description: Scopri come utilizzare il metodo create per inizializzare l'SDK Java e creare un'istanza di [!UICONTROL TargetClient] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.
feature: APIs/SDKs
exl-id: 0e0ddead-7de8-4549-b81c-e72598558e4b
source-git-commit: 1d080b5e402e5d55039bf06611b44678cc6c36de
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 17%

---

# Inizializzare l’SDK Java

## Descrizione

Utilizza il metodo `create` per inizializzare l&#39;SDK Java e creare un&#39;istanza di [!UICONTROL Target Client] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.

## Metodo

[!UICONTROL TargetClient] è stato creato utilizzando `TargetClient.create`.

### creare

```javascript {line-numbers="true"}
TargetClient TargetClient.create(ClientConfig clientConfig)
```

ClientConfig creato con `ClientConfig.builder`.

```javascript {line-numbers="true"}
ClientConfigBuilder ClientConfig.builder()
```

## Parametri

`ClientConfigBuilder` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- | --- | --- | --- | --- |
| client | Stringa | Sì | None (Nessuno) | [!UICONTROL Target Client Id] |
| organizationId | Stringa | Sì | None (Nessuno) | [!UICONTROL Experience Cloud Organization ID] |
| connectTimeout | Numero | No | 10000 | Timeout della connessione per tutte le richieste in millisecondi |
| socketTimeout | Numero | No | 10000 | Timeout socket per tutte le richieste in millisecondi |
| maxConnectionsPerHost | Numero | No | 100 | Numero massimo di connessioni per host [!DNL Target] |
| maxConnectionsTotal | Numero | No | 200 | Numero massimo di connessioni, inclusi tutti i [!DNL Target] host |
| connectionTtlMs | Numero | No | -1 | Il valore TTL (Total Time to Live) definisce la durata massima in millisecondi delle connessioni persistenti. Per impostazione predefinita, le connessioni rimarranno attive per un tempo indefinito |
| idleConnectionValidationMs | Numero | No | 1000 | Periodo di inattività in millisecondi dopo il quale le connessioni persistenti vengono riconvalidate prima del riutilizzo |
| evictIdleConnectionsAfterSecs | Numero | No | 20 | Tempo in secondi per eliminare le connessioni inattive dal pool di connessioni |
| enableRetries | Booleano | No | true | Tentativi automatici per timeout socket (massimo 4) |
| logRequests | Booleano | No | false | Registra le richieste e le risposte [!DNL Target] nel debug |
| logRequestStatus | Booleano | No | false | Registra il tempo di risposta, lo stato e l&#39;URL [!DNL Target] |
| serverDomain | Stringa | No | `*client*.tt.omtrdc.net` | Sostituisce il nome host predefinito |
| protetto | Booleano | No | true | Annulla l&#39;impostazione per applicare lo schema HTTP |
| requestInterceptor | HttpRequestInterceptor | No | Nullo | Aggiungi intercettore di richiesta personalizzato |
| defaultPropertyToken | Stringa | No | None (Nessuno) | Imposta il token di proprietà predefinito per ogni chiamata `getOffers`. **Per le decisioni sul dispositivo**, l&#39;SDK scaricherà solo l&#39;artefatto che contiene le attività qualificate per il token di proprietà impostato in `defaultPropertyToken` |
| defaultDecisioningMethod | Enum DecisioningMethod | No | LATO_SERVER | Deve essere impostato su ON_DEVICE o HYBRID per abilitare le decisioni sul dispositivo |
| telemetryEnabled | Booleano | No | true | Consente ai clienti di rinunciare alla raccolta di dati aggiuntivi durante le richieste ai server [!DNL Target] |
| proxyConfig | ClientProxyConfig | No | None (Nessuno) | Consente al cliente di fornire i propri dettagli proxy |
| exceptionHandler | TargetExceptionHandler | No | None (Nessuno) | Può essere utilizzato per implementare la gestione delle eccezioni personalizzate durante l&#39;elaborazione delle regole |
| httpClient | HttpClient | No | None (Nessuno) | Consente di sostituire il client HTTP [!DNL Target] con un client HTTP personalizzato |
| onDeviceEnvironment | Stringa | No | produzione | Può essere utilizzato per specificare un ambiente su dispositivo diverso, ad esempio staging |
| onDeviceConfigHostname | Stringa | No | `assets.adobetarget.com` | Può essere utilizzato per specificare un host diverso da utilizzare per scaricare il file dell’artefatto di decisioning sul dispositivo |
| onDeviceDecisioningPollingIntSecs | int | No | 300 (5 minuti) | Numero di secondi tra i recuperi del file dell’artefatto di decisioning sul dispositivo |
| onDeviceArtifactPayload | byte[] | No | None (Nessuno) | Fornisce decisioni sul dispositivo con precedente payload dell’artefatto per consentire l’esecuzione immediata |
| onDeviceDecisioningHandler | OnDeviceDecisioningHandler | No | None (Nessuno) | Registra i callback per gli eventi di decisioning sul dispositivo |
| onDeviceAllMatchingRulesMboxes | Elenco\&lt;Stringa\> | No | None (Nessuno) | Consente agli utenti di specificare le mbox per le quali verrà restituito tutto il contenuto della regola corrispondente durante il decisioning sul dispositivo |

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
