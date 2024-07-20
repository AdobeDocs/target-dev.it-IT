---
title: Autorizzazioni utente e proprietà
description: Gli SDK di  [!DNL Target]  includono il supporto per le autorizzazioni utente e le proprietà.
exl-id: 612faf1a-e8f9-4321-b831-90fba69ead3a
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '114'
ht-degree: 0%

---

# Autorizzazioni utente e proprietà

Gli SDK [!DNL Target] includono il supporto per le autorizzazioni utente e le proprietà. Se non sai in che modo [!DNL Adobe Target] gestisce le autorizzazioni Enterprise tramite aree di lavoro e proprietà, puoi trovare ulteriori informazioni in [Autorizzazioni per gli utenti Enterprise](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=it).

Il client può utilizzare un token di proprietà in uno dei due modi seguenti.

## Token proprietà globale

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    propertyToken: "8c4630b1-16db-e2fc-3391-8b3d81436cfb"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({...})
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("emeaprod4")
    .organizationId("0DD934B85278256B0A490D44@AdobeOrg")
    .defaultPropertyToken("8c4630b1-16db-e2fc-3391-8b3d81436cfb")
    .build();

TargetClient targetClient = TargetClient.create(clientConfig);
```

>[!ENDTABS]

## Token di proprietà accidentale nella chiamata getOffers

È inoltre possibile specificare un token di proprietà in una singola chiamata `getOffers`. A questo scopo, aggiungi un oggetto proprietà alla richiesta. Un token di proprietà specificato in questo modo ha la precedenza su un set nella configurazione.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
    request: {
        execute: {
            pageLoad: {}
        },
        property: {
            token: "8c4630b1-16db-e2fc-3391-8b3d81436cfb"
        }           
    }
})
```

>[!TAB Java]

```java {line-numbers="true"}
ExecuteRequest executeRequest = new ExecuteRequest()
    .mboxes(getMboxRequests(mbox));

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
    .context(getContext(request))
    .execute(executeRequest)
    .cookies(getTargetCookies(request.getCookies()))
    .property(new Property().token("8c4630b1-16db-e2fc-3391-8b3d81436cfb"))
    .build();

TargetDeliveryResponse targetResponse = targetClient.getOffers(targetDeliveryRequest);
```

>[!ENDTABS]
