---
title: Invia notifiche di visualizzazione o clic a [!DNL Adobe Target] utilizzo dell’SDK Python
description: Scopri come utilizzare sendNotifications() per inviare notifiche di visualizzazione o di clic a [!DNL Adobe Target] per la misurazione e il reporting.
feature: APIs/SDKs
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 8%

---

# Invia notifiche (Python)

## Descrizione

`send_notifications()` viene utilizzato per inviare notifiche di visualizzazione o clic a [!DNL Adobe Target] per la misurazione e il reporting.

>[!NOTE]
>
>Quando un `execute` L&#39;oggetto con i parametri richiesti si trova all&#39;interno della richiesta stessa. L&#39;impression verrà incrementata automaticamente per le attività qualificate.

I metodi SDK che incrementano automaticamente un’impression sono:

* `get_offers()`
* `get_attributes()`

Quando un `prefetch` viene passato all&#39;interno della richiesta, l&#39;impression non viene incrementata automaticamente per le attività con mbox all&#39;interno di `prefetch` oggetto. `Send_notifications()` deve essere utilizzato per le esperienze preacquisite per incrementare impressioni e conversioni.

## Metodo

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## Parametri

`options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | impostazione predefinita | Descrizione |
| --- | --- | --- | --- | --- |
| richiesta | DeliveryRequest | Sì | None (Nessuno) | Conforme al [[!UICONTROL API di consegna di Target]](/help/dev/implement/delivery-api/overview.md) richiesta |
| target_cookie | str | no | None (Nessuno) | [!DNL Target] cookie |
| target_location_hint | str | no | None (Nessuno) | [!DNL Target] hint di posizione |
| consumer_id | str | no | None (Nessuno) | Quando si uniscono più chiamate, è necessario fornire diversi ID consumer |
| customer_ids | list[CustomerId] | no | None (Nessuno) | Elenco di ID cliente in formato compatibile con VisitorId |
| session_id | str | no | None (Nessuno) | Utilizzato per collegare più richieste |
| callback | richiamabile | no | None (Nessuno) | Se gestisci la richiesta in modo asincrono, il callback viene richiamato quando la risposta è pronta |
| err_callback | richiamabile | no | None (Nessuno) | Se gestisci la richiesta in modo asincrono, il callback di errore viene richiamato quando viene generata l&#39;eccezione |

## Restituisce

`Returns` a `TargetDeliveryResponse` se chiamato in modo sincrono (impostazione predefinita), oppure `AsyncResult` se chiamato con un callback. `TargetDeliveryResponse` ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| risposta | DeliveryResponse | Conforme al [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) risposta |
| target_cookie | dict | [!DNL Target] cookie |
| target_location_hint_cookie | dict | [!DNL Target] cookie hint posizione |
| dettagli_analisi | list[AnalyticsResponse] | [!DNL Analytics] payload, in caso di lato client [!DNL Analytics] utilizzo |
| traccia |  | list[dict] | Dati di trace aggregati per tutte le mbox/visualizzazioni di richiesta |
| response_tokens | list[dict] | Un elenco di [&#x200B;Token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) |
| meta | dict | Metadati decisionali aggiuntivi da utilizzare con le decisioni sul dispositivo |

## Esempio

Innanzitutto, creiamo il [!UICONTROL API di consegna di Target] richiesta di preacquisizione del contenuto per `home` e `product1` mbox.

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

Una risposta corretta conterrà [!UICONTROL API di consegna di Target] oggetto response, che contiene contenuto preacquisito per le mbox richieste. Un esempio `target_response["response"]` L&#39;oggetto (formattato come dict) può essere visualizzato come segue:

### Python

```python {line-numbers="true"}
{
  "status": 200,
  "requestId": "e8ac2dbf5f7d4a9f9280f6071f24a01e",
  "id": {
    "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "adobetargetmobile",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "home",
        "options": [
          {
            "type": "html",
            "content": "HOME OFFER",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      },
      {
        "index": 1,
        "name": "product1",
        "options": [
          {
            "type": "html",
            "content": "TEST OFFER 1",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      }
    ]
  }
}
```

Osserva la mbox `name` e `state` campi, nonché `eventToken` in ciascuna delle opzioni di contenuto di Target. Questi devono essere forniti nella sezione `send_notifications()` non appena viene visualizzata ogni opzione di contenuto. Supponiamo che `product1` mbox è stato visualizzato su un dispositivo non browser. La richiesta di notifica verrà visualizzata come segue:

### Python

```python {line-numbers="true"}
notification_mbox = NotificationMbox(name="product1",
                                     state="J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
notification = Notification(
    id="1",
    type=MetricType.DISPLAY,
    timestamp=1621530726000,  # Epoch time in milliseconds
    mbox=notification_mbox,
    tokens=["t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="]
)
notification_request = DeliveryRequest(notifications=[notification])
```

Nota che abbiamo incluso sia lo stato mbox che il token evento corrispondente al [!DNL Target] offerta consegnata nella risposta di preacquisizione. Dopo aver generato la richiesta di notifiche, possiamo inviarla a [!DNL Target] tramite `send_notifications()` Metodo API:

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
