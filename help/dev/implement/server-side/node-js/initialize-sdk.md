---
title: Inizializzare l’SDK di Node.js utilizzando il metodo create
description: Scopri come utilizzare il metodo create per inizializzare l'SDK di Node.js e creare un'istanza del client  [!DNL Target]  per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.
feature: APIs/SDKs
exl-id: 71516e44-508a-4d8d-9f2b-7c54243e9c60
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 18%

---

# Inizializzare l’SDK di Node.js

## Descrizione

Utilizza il metodo `create` per inizializzare l&#39;SDK di Node.js e creare un&#39;istanza del client [!UICONTROL Target] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.

## Metodo

**crea**

```js {line-numbers="true"}
TargetClient.create(options: Object): TargetClient
```

## Parametri

`options` ha la seguente struttura:

| Nome | Tipo | Obbligatorio | Predefinito | Descrizione |
| --- | --- | --- | --- | --- |
| client | Stringa | Sì | None (Nessuno) | [!UICONTROL Adobe Target Client ID] |
| organizationId | Stringa | Sì | None (Nessuno) | [!UICONTROL Experience Cloud Organization ID] |
| ambiente | Stringa | No | produzione | Nome dell’ambiente di destinazione. Nell&#39;interfaccia utente [!DNL Target], [!UICONTROL Administration] > [!UICONTROL Environments]. |
| timeout | Numero | No | 3000 | Timeout in millisecondi |
| serverDomain | Stringa | No | `*client*.tt.omtrdc.net` | Sostituisce il nome host predefinito |
| protetto | Booleano | No | true | Annulla l&#39;impostazione per applicare lo schema HTTP |
| logger | Oggetto | No | Registratore NOOP | Sostituisce il logger NOOP predefinito |
| targetLocationHint | Stringa | No | None (Nessuno) | Hint posizione destinazione |
| fetchApi | Funzione | No | global.fetch o window.fetch | [fetch](https://fetch.spec.whatwg.org/) è utilizzato dall&#39;SDK per le richieste http. Per impostazione predefinita viene utilizzato il node-fetch o l’implementazione del browser Fetch. Tuttavia, è possibile fornire un&#39;implementazione alternativa utilizzando `fetchApi` |
| propertyToken | Stringa | No | None (Nessuno) | **Token proprietà destinazione**. Se specificato qui, tutte le chiamate `getOffers` utilizzeranno questo valore. **Per le decisioni sul dispositivo**, l&#39;SDK scaricherà solo l&#39;artefatto che contiene le attività qualificate per il token di proprietà impostato in `propertyToken` |
| decisioningMethod | Stringa | No | lato server | Determina il metodo decisionale da utilizzare ([sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), lato server, ibrido) |
| pollingInterval | Numero | No | 300000 (5 minuti) | Intervallo di polling per l&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) (in millisecondi) |
| artifactLocation | Stringa | No | None (Nessuno) | URL completo dell&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Sostituisce la posizione determinata internamente. |
| artifactPayload | Oggetto | No | None (Nessuno) | Payload JSON dell&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Se specificato, viene utilizzato invece di richiederne uno da un URL. |
| [eventi](sdk-events.md) | Oggetto&lt;String,Funzione> | No | None (Nessuno) | Oggetto facoltativo con chiavi di nome evento e valori della funzione di callback |
| telemetryEnabled | Booleano | No | true | Quando è abilitato, Adobe raccoglie i dati di telemetria relativi all’utilizzo delle funzioni SDK e alle prestazioni. I dati personali non vengono raccolti. |

## Esempio

### Node.js

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    events: {clientReady: targetClientReady }
};

const targetClient = TargetClient.create(CONFIG);

function targetClientReady() {
    // make calls to Adobe Target
}
```
