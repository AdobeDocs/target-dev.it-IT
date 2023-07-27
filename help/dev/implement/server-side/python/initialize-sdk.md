---
title: Inizializzare l’SDK di Python utilizzando il metodo create
description: Scopri come utilizzare il metodo create per inizializzare l’SDK di Python e creare un’istanza del [!UICONTROL TargetClient] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.
feature: APIs/SDKs
exl-id: 3e231e8e-696d-45c7-b733-79bf99da5bec
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 18%

---

# Inizializzare l’SDK Python

Descrizione Utilizza il `create` per inizializzare l&#39;SDK di Python e creare un&#39;istanza del [!UICONTROL Client di destinazione] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.

## Metodo

### creare

```python {line-numbers="true"}
TargetClient.create(options)
```

## Parametri

`options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | impostazione predefinita | Descrizione |
| --- | --- | --- | --- | --- |
| client | str | Sì | None (Nessuno) | [!UICONTROL ID client Adobe Target] |
| organization_id | str | Sì | None (Nessuno) | [!UICONTROL ID organizzazione Experience Cloud] |
| timeout | int | No | 3000 | Timeout in millisecondi |
| server_domain | str | No | `client.tt.omtrdc.net` |  | Sostituisce il nome host predefinito |
| protetto | booleano | No | true | Annulla l&#39;impostazione per applicare lo schema HTTP |
| logger | oggetto | No | Registratore INFO |  | Sostituisce il logger INFO predefinito |
| target_location_hint | str | No | None (Nessuno) | [!DNL Target] hint di posizione |
| property_token | str | No | None (Nessuno) | [!DNL Target] Token di proprietà. Se specificato qui, tutte le chiamate get_offers utilizzeranno questo valore. |
| decisioning_method | str | No | lato server | Determina il metodo decisionale da utilizzare ([su dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), lato server, ibrido) |
| polling_interval | int | No | 300000 (5 minuti) | Intervallo di polling per [artefatto della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) (in ms) |
| artifact_location | str | No | None (Nessuno) | Un URL completo per il [artefatto della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Sostituisce la posizione determinata internamente. |
| artifact_payload | oggetto | No | None (Nessuno) | Il payload JSON del [artefatto della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Se specificato, viene utilizzato invece di richiederne uno da un URL. |
| [events](sdk-events.md) | dict &lt;str callable=&quot;&quot;> | No | None (Nessuno) | Oggetto facoltativo con chiavi di nome evento e valori della funzione di callback |
| environment_id | int | No | produzione | Il [!DNL Target] ID ambiente |
| ambiente | str | No | produzione | Il [!DNL Target] nome dell’ambiente |
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
