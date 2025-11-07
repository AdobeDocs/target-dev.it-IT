---
title: Inizializzare Python SDK utilizzando il metodo create
description: Scopri come utilizzare il metodo create per inizializzare Python SDK e creare un'istanza di [!UICONTROL TargetClient] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.
feature: APIs/SDKs
exl-id: 3e231e8e-696d-45c7-b733-79bf99da5bec
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 15%

---

# Inizializzare Python SDK

Descrizione
Utilizzare il metodo `create` per inizializzare Python SDK e creare un&#39;istanza di [!UICONTROL Target Client] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.

## Metodo

### creare

```python {line-numbers="true"}
TargetClient.create(options)
```

## Parametri

`options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- | --- | --- | --- | --- |
| client | str | Sì | None (Nessuno) | [!UICONTROL Adobe Target client ID] |
| organization_id | str | Sì | None (Nessuno) | [!UICONTROL Experience Cloud Organization ID] |
| timeout | int | No | 3000 | Timeout in millisecondi |
| server_domain | str | No | `client.tt.omtrdc.net` | Sostituisce il nome host predefinito |
| protetto | booleano | No | true | Annulla l&#39;impostazione per applicare lo schema HTTP |
| logger | oggetto | No | Registratore INFO | Sostituisce il logger INFO predefinito |
| target_location_hint | str | No | None (Nessuno) | [!DNL Target] hint di posizione |
| property_token | str | No | None (Nessuno) | Token di proprietà [!DNL Target]. Se specificato qui, tutte le chiamate get_offers utilizzeranno questo valore. |
| decisioning_method | str | No | lato server | Determina il metodo decisionale da utilizzare ([sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), lato server, ibrido) |
| polling_interval | int | No | 300000 (5 minuti) | Intervallo di polling per l&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) (in ms) |
| artifact_location | str | No | None (Nessuno) | URL completo dell&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Sostituisce la posizione determinata internamente. |
| artifact_payload | oggetto | No | None (Nessuno) | Payload JSON dell&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Se specificato, viene utilizzato invece di richiederne uno da un URL. |
| [eventi](sdk-events.md) | dict &lt;str, chiamabile> | No | None (Nessuno) | Oggetto facoltativo con chiavi di nome evento e valori della funzione di callback |
| environment_id | int | No | produzione | ID ambiente [!DNL Target] |
| ambiente | str | No | produzione | Nome dell&#39;ambiente [!DNL Target] |
| cdn_environment | str | No | produzione | Nome dell’ambiente CDN |
| telemetria_abilitata | booleano | No | Vero | Se è impostato su False, i dati di telemetria non verranno inviati a [!DNL Adobe] |
| version | str | No | None (Nessuno) | Numero di versione di questo SDK |

## Esempio

### Python

```python {line-numbers="true"}
from target_python_sdk import TargetClient

def client_ready_callback(ready_event):
    # make calls to Adobe Target

client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
