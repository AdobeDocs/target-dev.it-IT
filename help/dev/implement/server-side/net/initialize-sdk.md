---
title: Inizializzare .NET SDK utilizzando il metodo create
description: Scopri come utilizzare il metodo create per inizializzare Java SDK e creare un'istanza di [!UICONTROL TargetClient] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.
feature: APIs/SDKs
exl-id: 501010c3-22f4-49a8-b2ac-c7307232d180
TQID: https://experienceleague.adobe.com/uOEojoWWjXmcDl2yY1UmSRD-EXL0j9p-p-eE8PXa7Rk
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: b18c88053a47a97d6718a69cb72cb4e5d99969c8
workflow-type: tm+mt
source-wordcount: 369
ht-degree: 14%

---

# Inizializzare .NET SDK

## Descrizione

Utilizzare il metodo `Create` per inizializzare .NET SDK e creare un&#39;istanza del client [!UICONTROL Target] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.

Quando si utilizza l&#39;iniezione di dipendenze .NET, è sufficiente aggiungere il passaggio di configurazione SDK at service chiamando `services.AddTargetLibrary()`;, quindi inserire `ITargetClient targetClient` nel costruttore dell&#39;app.

In seguito, utilizzare il metodo `Initialize` di SDK per configurare SDK, completando così il passaggio di inizializzazione.

## Metodo

`TargetClient` è stato creato utilizzando `TargetClient.Create`.

## C#

```csharp {line-numbers="true"}
TargetClient TargetClient.Create(TargetClientConfig clientConfig)
```

`ClientConfig` è stato creato con ClientConfig.Builder.

## C#

```csharp {line-numbers="true"}
TargetClientConfig.Builder TargetClientConfig.Builder()
```

## Parametri

`TargetClientConfig.Builder` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- | --- | --- | --- | --- |
| Client | stringa | Sì | None (Nessuno) | [!UICONTROL ID client di destinazione] |
| OrganizationId | stringa | Sì | None (Nessuno) | [!UICONTROL ID organizzazione Experience Cloud] |
| Timeout | int | No | 10000 | Timeout per tutte le richieste in millisecondi |
| Proxy | WebProxy | No | nulle | Proxy per tutte le [!DNL Target] richieste |
| Criterio nuovo tentativo | Policy | No | nulle | Criterio per nuovo tentativo per tutte le [!DNL Target] richieste |
| AsyncRetryPolicy | AsyncPolicy | No | nulle | Criteri per nuovi tentativi asincroni per tutte le [!DNL Target] richieste |
| Logger | ILogger | No | nulle | Utilizzato per la registrazione di debug di [!DNL Target] richieste e risposte |
| ServerDomain | stringa | No | `client.tt.omtrdc.net` | Sostituisce il nome host predefinito |
| Protetto | booleano | No | true | Annulla l&#39;impostazione per applicare lo schema HTTP |
| DefaultPropertyToken | stringa | No | nulle | Imposta il token di proprietà predefinito per ogni chiamata `getOffers` |
| TelemetriaAbilitata | booleano | No | true | Inviare dati di telemetria per migliorare l&#39;esperienza di utilizzo di SDK |
| DecisioningMethod | Enum DecisioningMethod | No | Lato server | Deve essere impostato su OnDevice o su Hybrid per abilitare le decisioni su dispositivo |
| OnDeviceDecisioningReady | Azione | No | nulle | Delega per l’evento &quot;on-device decisioning Ready&quot; (chiamato una volta quando le decisioni sul dispositivo sono pronte) |
| ArtefattoDownloadRiuscito | Azione | No | nulle | Delegato per il completamento del download dell’artefatto di decisioning sul dispositivo (chiamato a ogni download dell’artefatto riuscito) |
| ArtifactDownloadFailed | Azione | No | nulle | Delegato per errore di download dell’artefatto di decisioning sul dispositivo (chiamato a ogni download dell’artefatto non riuscito) |
| OnDeviceEnvironment | stringa | No | produzione | Può essere utilizzato per specificare un ambiente su dispositivo diverso, ad esempio staging |
| OnDeviceConfigHostname | stringa | No | `assets.adobetarget.com` | Può essere utilizzato per specificare un host diverso da utilizzare per scaricare il file dell’artefatto di decisioning sul dispositivo |
| OnDeviceDecisioningPollingIntSecs | int | No | 300 (5 min.) | Numero di secondi tra i recuperi del file dell’artefatto di decisioning sul dispositivo |
| OnDeviceArtifactPayload | stringa | No | nulle | Fornisce decisioni sul dispositivo con un payload dell’artefatto locale per consentire l’esecuzione immediata |

## Esempio

## C#

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeclient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

targetClient = TargetClient.Create(targetClientConfig);

// make calls to Adobe Target
```
