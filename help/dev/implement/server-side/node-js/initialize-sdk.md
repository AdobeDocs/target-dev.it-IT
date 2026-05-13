---
title: Inizializzare il SDK Node.js utilizzando il metodo create
description: Scopri come utilizzare il metodo create per inizializzare il SDK Node.js e creare un'istanza del client  [!DNL Target]  per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.
feature: APIs/SDKs
exl-id: 71516e44-508a-4d8d-9f2b-7c54243e9c60
TQID: https://experienceleague.adobe.com/uawle0-l5bcv-FuXMLkPc8kIf8DvbkRqAYelr-ehNLk
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 321
ht-degree: 18%

---

# Inizializzare il SDK di Node.js

## Descrizione

Utilizza il metodo `create` per inizializzare il SDK Node.js e creare un&#39;istanza del client [!UICONTROL Target] per effettuare chiamate a [!DNL Adobe Target] per esperimenti ed esperienze personalizzate.

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
| fetchApi | Funzione | No | global.fetch o window.fetch | [fetch](https://fetch.spec.whatwg.org/) è utilizzato da SDK per le richieste http. Per impostazione predefinita viene utilizzato il node-fetch o l’implementazione del browser Fetch. Tuttavia, è possibile fornire un&#39;implementazione alternativa utilizzando `fetchApi` |
| propertyToken | Stringa | No | None (Nessuno) | **Token proprietà destinazione**. Se specificato qui, tutte le chiamate `getOffers` utilizzeranno questo valore. **Per le decisioni sul dispositivo**, SDK scaricherà solo l&#39;artefatto che contiene le attività qualificate per il token di proprietà impostato in `propertyToken` |
| decisioningMethod | Stringa | No | lato server | Determina il metodo decisionale da utilizzare ([sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md), lato server, ibrido) |
| pollingInterval | Numero | No | 300000 (5 minuti) | Intervallo di polling per l&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) (in millisecondi) |
| artifactLocation | Stringa | No | None (Nessuno) | URL completo dell&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Sostituisce la posizione determinata internamente. |
| artifactPayload | Oggetto | No | None (Nessuno) | Payload JSON dell&#39;artefatto [della regola di decisioning sul dispositivo](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). Se specificato, viene utilizzato invece di richiederne uno da un URL. |
| [eventi](sdk-events.md) | Oggetto&lt;String,Funzione> | No | None (Nessuno) | Oggetto facoltativo con chiavi di nome evento e valori della funzione di callback |
| telemetryEnabled | Booleano | No | true | Quando è abilitata, Adobe raccoglierà i dati di telemetria relativi all’utilizzo delle funzioni e alle prestazioni di SDK. I dati personali non vengono raccolti. |

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
