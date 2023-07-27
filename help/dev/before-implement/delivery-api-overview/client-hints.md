---
title: Hint client API di consegna Adobe Target
description: Come si utilizzano gli Client Hints in [!DNL Adobe Target] API di consegna?
exl-id: 317b9d7d-5b98-464e-9113-08b899ee1455
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Client Hints e [!UICONTROL API di consegna di Adobe Target]

Gli Client Hints devono essere inviati a [!DNL Adobe Target] nella richiesta di offerte.

In genere, si consiglia di inviare tutti gli Client Hints disponibili a [!DNL Target]. Per ulteriori informazioni, consulta [User-agent e Client Hints](/help/dev/implement/client-side/atjs/user-agent-and-client-hints.md) nel [Implementazione lato client](../../implement/client-side/overview.md) sezione.

## Chiamate dirette API di consegna

### Dal browser

In questo caso, il browser invia Client Hints a bassa entropia a [!DNL Target] automaticamente tramite intestazioni di richiesta. Tuttavia, questa implementazione presenta alcune limitazioni a livello di browser. Primo: dal browser non verranno inviate intestazioni dei Client Hints, a meno che la richiesta non venga effettuata tramite https. Secondo - Client Hints non verranno inviati alla prima richiesta a [!DNL Target] sulla pagina. Le intestazioni dei Client Hints vengono inviate solo alla seconda richiesta e successivamente a tutte le richieste. Ciò significa che la segmentazione e la personalizzazione del pubblico non possono essere eseguite da [!DNL Target] alla prima visita di pagina. Per ovviare a entrambe queste limitazioni, si consiglia vivamente di utilizzare l’API dei User Agent Client Hints nel browser per raccogliere direttamente i Client Hints e inviarli al payload della richiesta.

### Da un server

In questo caso, gli Client Hints devono essere inoltrati manualmente dal browser a [!DNL Target] nella richiesta API di consegna.

```
curl -X POST 'http://mboxedge28.tt.omtrdc.net/rest/v1/delivery?client=myClientCode&sessionId=abcdefghijkl00014' -d '{
  "context": {
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36",
    "clientHints": {
      "Sec-CH-UA-Model": "iPhone",
      "Sec-CH-UA-Mobile": true,
      "Sec-CH-UA-Platform": "iOS",
      "Sec-CH-UA": "[ { \"brand\": \"Chromium\", \"version\": \"91\" }, { \"brand\": \" Not;A Brand\", \"version\": \"99\" } ]",
      "Sec-CH-UA-Full-Version-List": "[ { \"brand\": \"Chromium\", \"version\": \"91.1.1.1\" }, { \"brand\": \" Not;A Brand\", \"version\": \"99.1.1.1\" } ]",
      "Sec-CH-UA-Platform-Version": "10.0.0",
      "Sec-CH-UA-Arch": "x86",
      "Sec-CH-UA-Bitness": "64"
    }
  },
  "execute": {
    "mboxes": [{
      "name": "home",
      "index": 1
    }]
  }
}'
```

## Formattazione

Le intestazioni dei Client Hints Sec-CH-UA e Sec-CH-UA-Full-Version-List hanno un formato diverso rispetto ai risultati dell’API del browser Client Hints (navigator.userAgentData.brands/navigator.userAgentData.getHighEntropyValues). Entrambi questi formati sono accettati dall’API di consegna. L’API di consegna normalizza i valori nel formato utilizzato nelle intestazioni della richiesta, importante da tenere presente se si accede agli Client Hints negli script di profilo.
