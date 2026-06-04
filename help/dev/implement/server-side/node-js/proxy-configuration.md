---
title: Implementa la configurazione proxy nel SDK  [!DNL Adobe Target] Node.js
description: Scopri come configurare la configurazione proxy [!UICONTROL TargetClient] in  [!DNL Adobe Target] Node.js SDK.
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
TQID: https://experienceleague.adobe.com/kaE-ZEOTteaVp5kWSHiVYCvEiHuQHSMqeWRq6r-mJaA
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 84
ht-degree: 0%

---

# Configurazione proxy (Node.js)

Per configurare un proxy per le richieste HTTP di Node SDK, sovrascrivi l’API di recupero utilizzata da SDK durante l’inizializzazione.

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
Visita l&#39;archivio degli esempi di [Node SDK](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration)
esempi di configurazione proxy per le versioni precedenti di node e ulteriori informazioni.
