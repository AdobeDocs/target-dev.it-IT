---
title: Iscriviti agli eventi in [!DNL Adobe Target] SDK Python
description: Scopri come abbonarti a vari eventi che si verificano nell’SDK di Python utilizzando [!UICONTROL OnDeviceDecisioningHandler] oggetto.
feature: APIs/SDKs
exl-id: 4e32e3b5-6072-4703-b09d-abb467aa1304
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 3%

---

# Eventi SDK (Python)

## Descrizione

Quando [inizializzazione dell’SDK](initialize-sdk.md), il `options["events"]` dict è un oggetto facoltativo con chiavi di nome evento e valori della funzione di callback. Può essere utilizzato per abbonarsi a vari eventi che si verificano all&#39;interno dell&#39;SDK. Ad esempio, `client_ready` L&#39;evento può essere utilizzato con una funzione di callback che verrà richiamata quando l&#39;SDK sarà pronto per le chiamate ai metodi.

Quando `callback` viene chiamata la funzione, viene passato un oggetto evento. Ogni evento ha una `type` corrisponde al nome dell’evento e alcuni eventi includono proprietà aggiuntive con informazioni pertinenti.

## Eventi

| Nome evento (tipo) | Descrizione | Proprietà evento aggiuntive |
| --- | --- | --- |
| client_ready | Emesso quando l’artefatto viene scaricato e l’SDK è pronto per le chiamate get_offers. Consigliato quando si utilizza | metodo di decisioning sul dispositivo. | None (Nessuno) |
| artifact_download_successfully | Viene emesso ogni volta che viene scaricato un nuovo artefatto. | artifact_payload, artifact_location |
| artifact_download_failed | Emesso ogni volta che un artefatto non viene scaricato. | artifact_location, errore |

## Esempio

### Python

```python {line-numbers="true"}
def client_ready_callback():
    # make get_offers requests

def artifact_download_succeeded(event):
    print("The artifact was successfully downloaded from {}".format(event.artifact_location))
    # optionally do something with event.artifact_payload, like persist it

def artifact_download_failed(event):
    print("The artifact failed to download from {} with the following error: {}"
          .format(event.artifact_location, str(event.error)))

client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback,
        "artifact_download_succeeded": artifact_download_succeeded,
        "artifact_download_failed": artifact_download_failed
    }
}
target_client = target_client.create(client_options)
```
