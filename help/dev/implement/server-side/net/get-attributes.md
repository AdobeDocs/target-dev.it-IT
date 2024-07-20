---
title: Usa getAttributes in [!DNL Adobe Target] con .NET SDK
description: Scopri come utilizzare getAttributes() per recuperare esperienze di sperimentazione e personalizzate da [!DNL Target]  ed estrarre valori di attributi.
feature: APIs/SDKs
exl-id: 808da83d-3077-468b-a2ad-e35c25905f7d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 8%

---

# Ottieni attributi (.NET)

## Descrizione

`GetAttributes()` viene utilizzato per recuperare esperienze di sperimentazione e personalizzate da [!DNL Target] ed estrarre i valori degli attributi.

## Metodo

### getAttributes

```dotnet {line-numbers="true"}
TargetAttributes TargetClient.GetAttributes(TargetDeliveryRequest targetRequest, params string[] mboxes)
```

## Parametri

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | No | nulle | La stessa richiesta [!DNL Target] utilizzata per [Ottieni offerte&#x200B;](get-offers.md) |
| mboxNames | stringa parametri[] | No | nulle | Matrice di parametri di nomi mbox |

## Risultato

Un oggetto `TargetAttributes` è stato restituito da `TargetClient.GetAttributes()` con le proprietà e i metodi seguenti:

| Proprietà/Metodo | Tipo restituito | Descrizione |
| --- | --- | --- |
| Risposta | TargetDeliveryResponse | Restituisce l&#39;oggetto di risposta normalmente restituito da [Get Offers](get-offers.md) |
| ToDictionary | IReadOnlyDictionary | Restituisce un dizionario di dizionari con coppie chiave-valore raggruppate per nomi mbox |
| ToMboxDictionary(mboxName) | IReadOnlyDictionary | Restituisce un dizionario con coppie di valori chiave per la mbox fornita |
| GetBoolean(mboxName, key, defaultValue) | booleano | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| GetString(mboxName, key, defaultValue) | stringa | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| GetInteger(mboxName, key, defaultValue) | int | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| GetDouble(mboxName, key, defaultValue) | doppio | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| GetValue(mboxName, key, defaultValue) | T | Restituisce il valore per il nome mbox e la chiave attributo specificati |

## Esempio

### \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .Build();

var offerAttributes = targetClient.GetAttributes(targetDeliveryRequest, "demo-engineering-flags");

//returns just the value of searchProviderId from the mbox offer
var searchProviderId = offerAttributes.GetString("demo-engineering-flags", "searchProviderId");

//returns a simple Dictionary representing the mbox offer
var engineeringFlags = offerAttributes.ToMboxDictionary("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

var assetUrl = $"http://{engineeringFlags["cdnHostname"]}/path/to/asset";
```
