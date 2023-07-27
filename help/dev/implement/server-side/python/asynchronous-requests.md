---
title: Come utilizzare le richieste asincrone in [!DNL Adobe Target] SDK Python
description: Scopri come [!DNL Target] L’SDK di Python supporta le richieste asincrone, che possono ridurre a zero il tempo di destinazione effettivo.
feature: APIs/SDKs
exl-id: 44ab74e5-3c1a-49cf-9fff-fe523b0c2592
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 4%

---

# Richieste asincrone (Python)

## Descrizione

Uno dei vantaggi dell&#39;integrazione lato server è la possibilità di sfruttare l&#39;enorme larghezza di banda e le risorse informatiche disponibili sul lato server utilizzando il parallelismo. [!DNL Target] L’SDK di Python supporta le richieste asincrone, che possono ridurre a zero il tempo di destinazione effettivo.

## Metodi supportati

### Python

```python {line-numbers="true"}
get_offers(options)
send_notifications(options)
get_attributes(mbox_names, options)
```

## Esempio

Un&#39;applicazione di esempio che utilizza `asyncio` Il modulo async/await in Python 3.9+ potrebbe essere simile al seguente:

### Python

```python {line-numbers="true"}
async def execute_mboxes(self, mboxes):
    context = Context(channel=ChannelType.WEB)
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_offers_options = {
      "request": delivery_request
    }
    return await asyncio.to_thread(target_client.get_offers, get_offers_options)

async def get_target_delivery_response(mboxes):
    target_delivery_response = await execute_mboxes(mboxes)
    response = Response(target_delivery_response.get("response").to_str(), status=200, mimetype='application/json')
    return response

mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
return asyncio.run(get_target_delivery_response(mboxes)
```

Questo esempio presuppone che tu stia utilizzando Python 3.9+. Se utilizzi una versione precedente di Python puoi comunque inviare richieste asincrone passando `options.callback` a `get_offers`. Controlla l’app Flask di esempio per ulteriori dettagli sull’esecuzione asincrona utilizzando i callback o asincrono/await. [qui](https://github.com/adobe/target-python-sdk/blob/main/samples/app.py).
