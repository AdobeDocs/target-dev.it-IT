---
title: Utilizzare getAttributes in [!DNL Adobe Target] con l’SDK Java
description: Scopri come utilizzare getAttributes() per recuperare esperienze di sperimentazione e personalizzate da [!DNL Target] ed estrarre i valori degli attributi.
feature: APIs/SDKs
exl-id: e493e1b9-7180-4a7c-b98d-be84cc3a57c3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 13%

---

# Ottieni attributi (Java)

## Descrizione

`getAttributes()` viene utilizzato per recuperare esperienze di sperimentazione e personalizzate da [!DNL Target] ed estrarre i valori degli attributi.

## Metodo

### getAttributes

```javascript {line-numbers="true"}
Attributes TargetClient.getAttributes(TargetDeliveryRequest targetRequest, String ...mboxes)
```

## Parametri

| Nome | Tipo | Obbligatorio | impostazione predefinita | Descrizione |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | Sì | None (Nessuno) | La stessa richiesta di destinazione utilizzata per [Ottieni offerte&#x200B;](get-offers.md) |
| mboxNames | array var-args | No | None (Nessuno) | Matrice var-args di nomi mbox |


## Risultato

Un `Attributes` l&#39;oggetto viene restituito da `TargetClient.getAttributes()` che ha i seguenti metodi:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| getBoolean(mboxName, key) | Booleano | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| getString(mboxName, key) | Stringa | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| getInteger(mboxName, key) | Intero | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| getDouble(mboxName, key) | Doppio | Restituisce il valore per il nome mbox e la chiave attributo specificati |
| toMboxMap(mboxName) | Mappa | Restituisce una mappa semplice con coppie chiave-valore |
| getResponse() | TargetDeliveryResponse | Restituisce l’oggetto di risposta normalmente restituito da getOffers |

## Esempio

### Java

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .build();

Attributes offerAttributes = targetJavaClient.getAttributes(targetDeliveryRequest, "demo-engineering-flags");

//returns just the value of searchProviderId from the mbox offer
String searchProviderId = offerAttributes.getString("demo-engineering-flags", "searchProviderId");

//returns a simple Map representing the mbox offer
Map<String, Object> engineeringFlags = offerAttributes.toMboxMap("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

String assetUrl = "http://" + engineeringFlags.cdnHostname + "/path/to/asset";
```
