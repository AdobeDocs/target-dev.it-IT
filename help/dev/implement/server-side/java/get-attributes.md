---
title: Usa getAttributes in [!DNL Adobe Target] con Java SDK
description: Scopri come utilizzare getAttributes() per recuperare esperienze di sperimentazione e personalizzate da [!DNL Target]  ed estrarre valori di attributi.
feature: APIs/SDKs
exl-id: e493e1b9-7180-4a7c-b98d-be84cc3a57c3
TQID: https://experienceleague.adobe.com/ZZy9nUXiyR-qwBmOgv-TPS6ZuilvAuW850gH1Doqquo
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 169
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

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | Sì | None (Nessuno) | La stessa richiesta di destinazione utilizzata per [Get Offers&#x200B;](get-offers.md) |
| mboxNames | array var-args | No | None (Nessuno) | Matrice var-args di nomi mbox |


## Risultato

Un oggetto `Attributes` è restituito da `TargetClient.getAttributes()` che dispone dei seguenti metodi:

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
