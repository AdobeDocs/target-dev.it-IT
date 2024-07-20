---
title: Implementa la configurazione proxy nell'SDK  [!DNL Adobe Target] Node.js
description: Scopri come configurare la configurazione proxy [!UICONTROL TargetClient] nell'SDK  [!DNL Adobe Target] Node.js.
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Configurazione proxy (Node.js)

Per configurare un proxy per le richieste HTTP dell’SDK del nodo, sovrascrivi l’API di recupero utilizzata dall’SDK durante l’inizializzazione.

Di seguito è riportato un esempio di base che mostra come eseguire l&#39;override di `fetchApi` durante l&#39;inizializzazione di `TargetClient` per aggiungere un proxy:

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

Si noti che questo funziona solo per le versioni di Nodo 18.2+, in cui `undici.fetch` è il `fetch` predefinito per il nodo.
Visita l&#39;archivio degli esempi dell&#39;SDK del [nodo](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration)
esempi di configurazione proxy per le versioni precedenti di node e ulteriori informazioni.
