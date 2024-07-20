---
title: Invia notifiche di visualizzazione o clic a  [!DNL Adobe Target]  tramite l'SDK Python
description: Scopri come utilizzare sendNotifications() per inviare notifiche di visualizzazione o clic a  [!DNL Adobe Target]  per la misurazione e il reporting.
feature: APIs/SDKs
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 8%

---

# Invia notifiche (Python)

## Descrizione

`send_notifications()` viene utilizzato per inviare notifiche di visualizzazione o clic a [!DNL Adobe Target] per la misurazione e il reporting.

>[!NOTE]
>
>Quando un oggetto `execute` con parametri obbligatori si trova all&#39;interno della richiesta stessa, l&#39;impression verrà incrementata automaticamente per le attività idonee.

I metodi SDK che incrementano automaticamente un’impression sono:

* `get_offers()`
* `get_attributes()`

Quando un oggetto `prefetch` viene passato all&#39;interno della richiesta, l&#39;impression non viene incrementata automaticamente per le attività con mbox all&#39;interno dell&#39;oggetto `prefetch`. `Send_notifications()` deve essere utilizzato per le esperienze preacquisite per incrementare impressioni e conversioni.

## Metodo

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## Parametri

`options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- | --- | --- | --- | --- |
| richiesta | DeliveryRequest | Sì | None (Nessuno) | Conforme alla richiesta [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) |
| target_cookie | str | no | None (Nessuno) | Cookie [!DNL Target] |
| target_location_hint | str | no | None (Nessuno) | [!DNL Target] hint di posizione |
| consumer_id | str | no | None (Nessuno) | Quando si uniscono più chiamate, è necessario fornire diversi ID consumer |
| customer_ids | list[CustomerId] | no | None (Nessuno) | Elenco di ID cliente in formato compatibile con VisitorId |
| session_id | str | no | None (Nessuno) | Utilizzato per collegare più richieste |
| callback | richiamabile | no | None (Nessuno) | Se gestisci la richiesta in modo asincrono, il callback viene richiamato quando la risposta è pronta |
| err_callback | richiamabile | no | None (Nessuno) | Se gestisci la richiesta in modo asincrono, il callback di errore viene richiamato quando viene generata l&#39;eccezione |

## Restituisce

`Returns` un `TargetDeliveryResponse` se chiamato in modo sincrono (impostazione predefinita) o un `AsyncResult` se chiamato con un callback. `TargetDeliveryResponse` ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| risposta | DeliveryResponse | Conforme alla risposta [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) |
| target_cookie | dict | Cookie [!DNL Target] |
| target_location_hint_cookie | dict | Cookie dell&#39;hint di posizione [!DNL Target] |
| dettagli_analisi | list[AnalyticsResponse] | Payload [!DNL Analytics], in caso di utilizzo di [!DNL Analytics] lato client |
| traccia |  | list[dict] | Dati di trace aggregati per tutte le mbox/visualizzazioni di richiesta |
| response_tokens | list[dict] | Elenco di [&#x200B;token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) |
| meta | dict | Metadati decisionali aggiuntivi da utilizzare con le decisioni sul dispositivo |

## Esempio

Innanzitutto, compiliamo la richiesta [!UICONTROL Target Delivery API] per preacquisire il contenuto per le mbox `home` e `product1`.

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

In caso di esito positivo, la risposta conterrà un oggetto di risposta [!UICONTROL Target Delivery API] contenente contenuto prerecuperato per le mbox richieste. Un oggetto `target_response["response"]` di esempio (formattato come dict) potrebbe essere visualizzato come segue:

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

Prendere nota dei campi mbox `name` e `state`, nonché del campo `eventToken`, in ciascuna delle opzioni di contenuto di Target. Questi devono essere forniti nella richiesta `send_notifications()`, non appena viene visualizzata ogni opzione di contenuto. Supponiamo che la mbox `product1` sia stata visualizzata su un dispositivo non browser. La richiesta di notifica verrà visualizzata come segue:

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

Si noti che sono stati inclusi sia lo stato mbox che il token evento corrispondente all&#39;offerta [!DNL Target] consegnata nella risposta di preacquisizione. Dopo aver generato la richiesta di notifiche, è possibile inviarla a [!DNL Target] tramite il metodo API `send_notifications()`:

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
