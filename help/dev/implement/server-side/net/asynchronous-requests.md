---
title: Come utilizzare le richieste asincrone in  [!DNL Adobe Target] .NET SDK
description: Scopri come  [!DNL Target] Java SDK supporta le richieste asincrone, riducendo a zero il tempo di destinazione effettivo.
feature: APIs/SDKs
exl-id: fd36cc7b-a884-4e57-93c2-8aff8256109a
TQID: https://experienceleague.adobe.com/E9rNmPdXe7HYg7XlIffpC4opGM9X6fFoHK-u0oLI-XE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 91
ht-degree: 4%

---

# Richieste asincrone (.NET)

## Descrizione

Uno dei vantaggi dell&#39;integrazione lato server è la possibilità di sfruttare l&#39;enorme larghezza di banda e le risorse informatiche disponibili sul lato server utilizzando il parallelismo. [!DNL Target] .NET SDK supporta le richieste asincrone, semplificando l&#39;integrazione di [!DNL Target] nel flusso di lavoro asincrono esistente dell&#39;app.

## Metodi supportati

### \.NET

```dotnet {line-numbers="true"}
Task<TargetDeliveryResponse> GetOffersAsync(TargetDeliveryRequest request);
Task<TargetDeliveryResponse> SendNotificationsAsync(TargetDeliveryRequest request);
Task<TargetAttributes> GetAttributesAsync(TargetDeliveryRequest request, params string[] mboxes);
```

## Esempio

Di seguito è riportato un esempio di utilizzo asincrono dell’API SDK:

### \.NET

```dotnet {line-numbers="true"}
var deliveryRequest = new TargetDeliveryRequest.Builder()
    .SetExecute(new ExecuteRequest(mboxes: new List<MboxRequest> { new MboxRequest(index: 1, name: "a1-serverside-ab") }))
    .Build();

var response = await this.targetClient.GetOffersAsync(deliveryRequest);

var notificationRequest = new TargetDeliveryRequest.Builder()
    .SetSessionId(response.Request.SessionId)
    .SetTntId(response.Response?.Id?.TntId)
    .SetNotifications(new List<Notification>
        {
            new (id: "1", type: MetricType.Display, timestamp: DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
                mbox: new NotificationMbox("product1", "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"),
                tokens: new List<string> { "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" })
        })
    .Build();

var notificationResponse = await this.targetClient.SendNotificationsAsync(notificationRequest);
```

L&#39;esempio presuppone che tu abbia [inizializzato SDK](initialize-sdk.md).
