---
title: Implementa la configurazione proxy in  [!DNL Adobe Target] Java SDK
description: Scopri come configurare la configurazione proxy di TargetClient nel SDK Java  [!DNL Adobe Target] .
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
TQID: https://experienceleague.adobe.com/Vo8KrM-3AGIvoO-E-iAQcAPqzXE24BM30LX7ji5E2Nk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 170
ht-degree: 2%

---

# Configurazione proxy (Java)

## Proxy di base

Se l&#39;applicazione che esegue SDK richiede un proxy per accedere a Internet, `TargetClient` dovrà essere configurato con una configurazione proxy nel modo seguente.

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

Se è richiesta un&#39;autenticazione proxy, le credenziali possono essere passate come parametri al costruttore `ClientProxyConfig`, come nell&#39;esempio seguente. Questo funziona solo per l’autenticazione proxy semplice nome utente/password.

### Autenticazione proxy di base

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port,username,password))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## Decisioning sul dispositivo

Affinché le richieste possano recuperare l’artefatto delle regole, il proxy deve essere configurato in modo da non memorizzare la risposta nella cache. Tuttavia, se non è possibile configurare il meccanismo di memorizzazione in cache del proxy per tale richiesta, utilizza un’opzione di configurazione come soluzione alternativa per ignorare la cache a livello di proxy. Questa soluzione alternativa aggiunge l&#39;intestazione `Authorization` con un valore stringa vuoto alla richiesta rules, che dovrebbe indicare al proxy che la risposta non deve essere memorizzata nella cache.

Per abilitare questa soluzione alternativa, impostare quanto segue:

```java {line-numbers="true"}
ClientConfig.builder()
    .shouldArtifactRequestBypassProxyCache(true)
    .build();
```


