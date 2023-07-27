---
title: Scopri come configurare il client HTTP personalizzato
description: Scopri come configurare TargetClient utilizzando ClientConfig.builder().httpClient().
feature: APIs/SDKs
exl-id: 7615029c-b62d-4ed1-aadb-32e364c4c654
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 0%

---

# Configurazione client HTTP personalizzata (Java)

Se l’applicazione che esegue l’SDK richiede un client HTTP personalizzato, per abilitare funzioni quali la configurazione di SSL o l’aggiunta di intestazioni predefinite alle richieste `TargetClient` dovrà essere configurato utilizzando `ClientConfig.builder().httpClient()`:

## Configurazione client HTTP personalizzata di base

L&#39;SDK supporta attualmente i client HTTP che implementano `org.apache.http.client.HttpClient` di rete.

### Implementazione di base

```java {line-numbers="true"}
CloseableHttpClient httpClient = HttpClients.custom().build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## Configurazione client HTTP personalizzata con configurazione SSL

Di seguito è riportato un esempio di configurazione di SSL nel `TargetClient` personalizzando il `HttpClient` passato in `ClientConfig`. Il seguente frammento di codice utilizza le classi del `org.apache.http.conn.ssl` pacchetto per la configurazione SSL.

### Implementazione SSL

```java {line-numbers="true"}
SSLContext context = SSLContextBuilder.create().build();
SSLConnectionSocketFactory sslSocketFactory = new SSLConnectionSocketFactory(context);
CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(sslSocketFactory).build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```
