---
title: Implementare la configurazione proxy in [!DNL Adobe Target] SDK di Node.js
description: Scopri come configurare [!UICONTROL TargetClient] configurazione proxy in [!DNL Adobe Target] SDK di Node.js.
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---

# Configurazione proxy (Node.js)

Per configurare un proxy per le richieste HTTP dell’SDK del nodo, sovrascrivi l’API di recupero utilizzata dall’SDK durante l’inizializzazione.

Di seguito è riportato un esempio di base che mostra come eseguire l&#39;override `fetchApi` durante il `TargetClient` inizializzazione per aggiungere un proxy:

```javascript {line-numbers="true"}
const { ProxyAgent } = require("undici");

const proxyAgent = new ProxyAgent("your proxy address here");

const fetchImpl = (url, options) => {
  const fetchOptions = options;
  fetchOptions.dispatcher = proxyAgent;
  return fetch(url, fetchOptions);
};

client = TargetClient.create({
    ...,
    fetchApi: fetchImpl
});
```

Questo funziona solo per le versioni Nodo 18.2+, in cui `undici.fetch` è il valore predefinito `fetch` per il nodo.
Visita il sito [Archivio di esempi di SDK per nodi](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration)
esempi di configurazione proxy per le versioni precedenti di node e ulteriori informazioni.
