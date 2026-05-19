---
title: Scopri come configurare il client HTTP personalizzato
description: Scopri come configurare TargetClient utilizzando ClientConfig.builder().httpClient().
feature: APIs/SDKs
exl-id: 7615029c-b62d-4ed1-aadb-32e364c4c654
TQID: https://experienceleague.adobe.com/SwijRIrhqSG4Mlij4sBH9Kx8tRB-6Bo7eyMoUZREOW8
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 108
ht-degree: 0%

---

# Configurazione client HTTP personalizzata (Java)

Se l&#39;applicazione che esegue SDK richiede un client HTTP personalizzato, per abilitare funzionalità quali la configurazione SSL o l&#39;aggiunta di intestazioni predefinite alle richieste, `TargetClient` dovrà essere configurato utilizzando `ClientConfig.builder().httpClient()`:

## Configurazione client HTTP personalizzata di base

Al momento SDK supporta i client HTTP che implementano l&#39;interfaccia `org.apache.http.client.HttpClient`.

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

Ecco un esempio di come configurare SSL in `TargetClient` personalizzando `HttpClient` passato in `ClientConfig`. Il frammento di codice seguente utilizza le classi del pacchetto `org.apache.http.conn.ssl` per la configurazione SSL.

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
