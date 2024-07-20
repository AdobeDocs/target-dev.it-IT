---
title: API dei profili di Adobe Target
description: Scopri come utilizzare le API del profilo di Adobe Target per inviare i dati dei visitatori a  [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 480cbbbe-4822-48c3-80d4-53628dee57b0
source-git-commit: e2462d12cf58ab5a588c13a96df5e6abafb9d675
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 1%

---

# Panoramica di [!DNL Adobe Target Profiles API]

[!DNL Adobe Target] crea e gestisce un profilo per ogni singolo utente. Questo profilo è archiviato nel cluster Edge [!DNL Target] e viene aggiornato in tempo reale dopo ogni visita.

## Struttura di un profilo [!DNL Target]

Un profilo di Target è costituito dai seguenti oggetti:

| Oggetto | Dettagli |
| --- | --- |
| `clientcode` | Il codice client [!DNL Target] a cui è associato il profilo. |
| `visitorId` | Identificatore del profilo. Può essere `tntid`, `thirdpartyid` o `marketingcloudvisitorid`. |
| `modifiedAt` | La marca temporale dell’ultimo aggiornamento del profilo. |
| `profileAttributes` | Elenco di tutti gli attributi di profilo memorizzati come coppie chiave-valore su quel singolo profilo. |

### Struttura del profilo di esempio

```
{
    "client": "<your-tenant-name>",
    "visitorId": "a1-mbox3rdPartyId",
    "modifiedAt": "2017-08-18T17:53:39.003-04:00",
    "profileAttributes": {
        "insurance": {
            "value": "false",
            "modifiedAt": "2017-07-31T20:34:55.625-04:00"
        },
        "country": {
            "value": "france",
            "modifiedAt": "2017-07-31T14:26:30.879-04:00"
        },
        "checking": {
            "value": "true",
            "modifiedAt": "2017-07-31T20:34:55.625-04:00"
        },
        "user.memberlevel": {
            "value": "0.0",
            "modifiedAt": "2017-08-09T18:18:04.661-04:00"
        },
        "param1": {
            "value": "value1",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "param2": {
            "value": "value2",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "firstSessionStart": {
            "value": "1500648715286",
            "modifiedAt": "2017-07-21T10:51:55.286-04:00"
        },
        "entity.name": {
            "value": "my-entityName",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "entity.id": {
            "value": "my-entityId",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        }
    }
}
```
