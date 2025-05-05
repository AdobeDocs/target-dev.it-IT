---
title: Utilizzare getOffers() in [!DNL Adobe Target] con l'SDK Python
description: Scopri come utilizzare getOffers() per eseguire una decisione e recuperare un'esperienza da [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 9539b806-e070-430e-80cf-cf632ce3f207
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 12%

---

# Ottieni offerte (Python)

## Descrizione

`get_offers()` viene utilizzato per eseguire una decisione e recuperare un&#39;esperienza da [!DNL Adobe Target].


## Metodo

### getOffers

```python {line-numbers="true"}
target_client_instance.get_offers(options)
```

## Parametri

La dipendenza `options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- | --- | --- | --- | --- |
| richiesta | DeliveryRequest | Sì | None (Nessuno) | Conforme alla richiesta [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) |
| target_cookie | str | no | None (Nessuno) | Cookie [!DNL Target] |
| target_location_hint | str | no | None (Nessuno) | [!DNL Target] hint di posizione |
| consumer_id | str | no | None (Nessuno) | Quando si uniscono più chiamate, è necessario fornire diversi ID consumer |
| customer_ids | list[CustomerId] | no | None (Nessuno) | Elenco di ID cliente in formato compatibile con VisitorId |
| session_id | str | no | None (Nessuno) | Utilizzato per collegare più richieste |
| callback | richiamabile | no | None (Nessuno) | Se gestisci la richiesta in modo asincrono, il callback viene richiamato quando la risposta è pronta |
| err_callback | richiamabile | no | None (Nessuno) | Se gestisci la richiesta in modo asincrono, il callback di errore viene richiamato quando viene generata l&#39;eccezione |

## Restituisce

Restituisce un valore `TargetDeliveryResponse` se chiamato in modo sincrono (impostazione predefinita) o un valore `AsyncResult` se chiamato con un callback. `TargetDeliveryResponse` ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| risposta | DeliveryResponse | Conforme alla risposta [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) |
| target_cookie | dict | Cookie [!DNL Target] |
| target_location_hint_cookie | dict | Cookie dell&#39;hint di posizione [!DNL Target] |
| dettagli_analisi | list[AnalyticsResponse] | Payload di Analytics, in caso di utilizzo di Analytics lato client |
| traccia | list[dict] | Dati di trace aggregati per tutte le mbox/visualizzazioni di richiesta |
| response_tokens | list[dict] | Elenco di &#x200B;[Token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=it) |
| meta | dict | Metadati decisionali aggiuntivi da utilizzare con le decisioni sul dispositivo |

`target_cookie` e `target_location_hint_cookie` oggetti utilizzati per restituire dati al browser hanno la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| name | str | Nome cookie |
| value | qualsiasi | Valore cookie, il valore verrà convertito in stringa |
| max_age | int | `max_age option` è una comodità per impostare le scadenze relative all&#39;ora corrente in secondi |

L&#39;oggetto `meta` utilizzato per indicare lo stato della risposta di destinazione ha la seguente struttura:

| Nome | Tipo | Descrizione |
| --- | --- | --- |
| decisioning_method | str | Quale metodo di decisione è stato utilizzato: su dispositivo o lato server |
| remote_mboxes | list`[str]` | Quando il metodo di decisione è `on-device`, viene fornito un array di nomi mbox che non è stato possibile decidere completamente sul dispositivo. In altre parole, è necessaria una richiesta [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md). |
| remote_views | list`[str]` | Quando il metodo di decisione è su dispositivo, viene fornito un array di nomi di visualizzazione che non è stato possibile decidere completamente sul dispositivo. In altre parole, è necessaria una richiesta [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md). |

## Esempio

### Python

```python {line-numbers="true"}
def client_ready_callback():
    context = Context(channel=ChannelType.WEB)
    mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_offers_options = {
      "request": delivery_request
    }

    target_delivery_response = target_client.get_offers(get_offers_options)


client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
