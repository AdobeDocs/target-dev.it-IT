---
title: Inizializzare .NET SDK utilizzando il metodo create
description: Scopri come utilizzare il metodo create per inizializzare Java SDK e creare un’istanza di [!UICONTROL TargetClient] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.
feature: APIs/SDKs
exl-id: 501010c3-22f4-49a8-b2ac-c7307232d180
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 16%

---

# Inizializzare .NET SDK

## Descrizione

Utilizza il `Create` per inizializzare .NET SDK e creare un&#39;istanza di [!UICONTROL Client di destinazione] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.

Quando utilizzi .NET Dependency Injection, aggiungi l&#39;SDK al passaggio di configurazione del servizio chiamando `services.AddTargetLibrary()`;, quindi iniettare `ITargetClient targetClient` nel costruttore dell’app.

In seguito, utilizza `Initialize` dell&#39;SDK per configurare l&#39;SDK, completando così il passaggio di inizializzazione.

## Metodo

`TargetClient` viene creato utilizzando `TargetClient.Create`.

## C\
#

```csharp {line-numbers="true"}
TargetClient TargetClient.Create(TargetClientConfig clientConfig)
```

`ClientConfig` viene creato utilizzando ClientConfig.Builder.

## C\
#

```csharp {line-numbers="true"}
TargetClientConfig.Builder TargetClientConfig.Builder()
```

## Parametri

`TargetClientConfig.Builder` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | impostazione predefinita | Descrizione |
| --- | --- | --- | --- | --- |
| Client | stringa | Sì | None (Nessuno) | [!UICONTROL ID client di destinazione] |
| OrganizationId | stringa | Sì | None (Nessuno) | [!UICONTROL ID organizzazione Experience Cloud] |
| Timeout | int | No | 10000 | Timeout per tutte le richieste in millisecondi |
| Proxy |  | WebProxy | No | nulle | Proxy per tutti [!DNL Target] richieste |
| Criterio nuovo tentativo | Policy | No | nulle | Criterio per tutti i tentativi [!DNL Target] richieste |
| AsyncRetryPolicy | AsyncPolicy | No | nulle | Criterio per nuovi tentativi asincroni per tutti [!DNL Target] richieste |
| Logger | ILogger | No | nulle | Utilizzato per la registrazione di debug di [!DNL Target] richieste e risposte |
| ServerDomain | stringa | No | `client.tt.omtrdc.net` | Sostituisce il nome host predefinito |
| Protetto | booleano | No | true | Annulla l&#39;impostazione per applicare lo schema HTTP |
| DefaultPropertyToken | stringa | No | nulle | Imposta il token di proprietà predefinito per ogni `getOffers` chiamare |
| TelemetriaAbilitata | booleano | No | true | Inviare dati di telemetria per migliorare l’esperienza di utilizzo dell’SDK |
| DecisioningMethod | Enum DecisioningMethod | No | Lato server | Deve essere impostato su OnDevice o su Hybrid per abilitare le decisioni su dispositivo |
| OnDeviceDecisioningReady | Azione | No | nulle | Delega per l’evento &quot;on-device decisioning Ready&quot; (chiamato una volta quando le decisioni sul dispositivo sono pronte) |
| ArtefattoDownloadRiuscito | Azione | No | nulle | Delegato per il completamento del download dell’artefatto di decisioning sul dispositivo (chiamato a ogni download dell’artefatto riuscito) |
| ArtifactDownloadFailed | Azione | No | nulle | Delegato per errore di download dell’artefatto di decisioning sul dispositivo (chiamato a ogni download dell’artefatto non riuscito) |
| OnDeviceEnvironment | stringa | No | produzione | Può essere utilizzato per specificare un ambiente su dispositivo diverso, ad esempio staging |
| OnDeviceConfigHostname | stringa | No | `assets.adobetarget.com` | Può essere utilizzato per specificare un host diverso da utilizzare per scaricare il file dell’artefatto di decisioning sul dispositivo |
| OnDeviceDecisioningPollingIntSecs | int | No | 300 (5 min.) | Numero di secondi tra i recuperi del file dell’artefatto di decisioning sul dispositivo |
| OnDeviceArtifactPayload | stringa | No | nulle | Fornisce decisioni sul dispositivo con un payload dell’artefatto locale per consentire l’esecuzione immediata |

## Esempio

## C\
#

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeclient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

targetClient = TargetClient.Create(targetClientConfig);

// make calls to Adobe Target
```
