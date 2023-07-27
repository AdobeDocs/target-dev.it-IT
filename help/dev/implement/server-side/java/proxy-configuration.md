---
title: Implementare la configurazione proxy in [!DNL Adobe Target] SDK Java
description: Scopri come configurare la configurazione proxy di TargetClient nel [!DNL Adobe Target] SDK Java.
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 1%

---

# Configurazione proxy (Java)

## Proxy di base

Se l’applicazione che esegue l’SDK richiede un proxy per accedere a Internet, il `TargetClient` dovrà essere configurato con una configurazione proxy come segue.

### Configurazione proxy di base

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## Autenticazione

Se è richiesta l&#39;autenticazione proxy, le credenziali possono essere trasmesse come parametri al `ClientProxyConfig` costruttore, come nell’esempio seguente. Questo funziona solo per l’autenticazione proxy semplice nome utente/password.

### Autenticazione proxy di base

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port,username,password))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```
