---
title: Implementare la configurazione proxy nell'SDK Java  [!DNL Adobe Target]
description: Scopri come configurare la configurazione proxy di TargetClient nell'SDK Java [!DNL Adobe Target] .
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
source-git-commit: 59ab3f53e2efcbb9f7b1b2073060bbd6a173e380
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 1%

---

# Configurazione proxy (Java)

## Proxy di base

Se l&#39;applicazione che esegue l&#39;SDK richiede un proxy per accedere a Internet, `TargetClient` dovrà essere configurato con una configurazione proxy come segue.

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


