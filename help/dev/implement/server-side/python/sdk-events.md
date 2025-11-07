---
title: Iscriviti agli eventi in  [!DNL Adobe Target] Python SDK
description: Scopri come abbonarti a vari eventi che si verificano all'interno di Python SDK utilizzando l'oggetto [!UICONTROL OnDeviceDecisioningHandler].
feature: APIs/SDKs
exl-id: 4e32e3b5-6072-4703-b09d-abb467aa1304
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 3%

---

# Eventi SDK (Python)

## Descrizione

Quando [viene inizializzato SDK](initialize-sdk.md), il file dict `options["events"]` è un oggetto facoltativo con chiavi di nome evento e valori della funzione di callback. Può essere utilizzato per abbonarsi a vari eventi che si verificano all’interno di SDK. Ad esempio, l&#39;evento `client_ready` può essere utilizzato con una funzione di callback che verrà richiamata quando SDK sarà pronto per le chiamate ai metodi.

Quando viene chiamata la funzione `callback`, viene passato un oggetto evento. Ogni evento ha un `type` corrispondente al nome dell&#39;evento e alcuni eventi includono proprietà aggiuntive con informazioni pertinenti.

## Eventi

| Nome evento (tipo) | Descrizione | Proprietà evento aggiuntive |
| --- | --- | --- |
| client_ready | Viene emesso quando l’artefatto viene scaricato e SDK è pronto per le chiamate get_offers. Consigliato quando si utilizza il metodo di decisione sul dispositivo. | None (Nessuno) |
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
