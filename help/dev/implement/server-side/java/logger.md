---
title: Inizializza Java SDK [!DNL Adobe Target] per registrare le richieste
description: Scopri come registrare le richieste nel Java SDK [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
TQID: 'https://experienceleague.adobe.com/xvduuV6cjVJu-yIoaxCvbPE-ZttfEViuM8B7sVczAC0'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
    internal-label: Implementation
  - id: a19e8738-9679-599a-b83b-5f2f15f8e4d6
    internal-label: APIs/SDKs
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 4%
---
# Logger (Java)

## Descrizione

Quando [viene inizializzato SDK](initialize-sdk.md), nell&#39;oggetto `ClientConfig` sono disponibili diverse opzioni che possono essere impostate per registrare le richieste.

| Opzione | Descrizione |
| --- | --- |
| `logRequests` | Registra l’intero corpo della richiesta e il corpo della risposta. |
| `logRequestStatus` | Registra l’URL della richiesta, lo stato e il tempo di risposta. |

[!DNL Target] Java SDK utilizza la registrazione `slf4j`. È necessario fornire l&#39;implementazione del logger come `java.util.logging`, `logback` e `log4j`. Per ulteriori informazioni, consultare [https://www.slf4j.org/manual.html](https://www.slf4j.org/manual.html). Tutti i registri verranno stampati in `debug`.

## Esempio

Aggiungi la dipendenza `slf4j`.

>[!BEGINTABS]

>[!TAB Gradle]

### Gradle

```javascript {line-numbers="true"}
compile 'org.slf4j:slf4j-simple:2.0.0-alpha0'
```

>[!TAB Maven]

```javascript {line-numbers="true"}
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>2.0.0-alpha0</version>
</dependency>
```

>[!ENDTABS]

Abilita i registri `DEBUG` in base all&#39;implementazione e contrassegna i flag di registrazione delle richieste.

### Debug

```javascript {line-numbers="true"}
System.setProperty(SimpleLogger.DEFAULT_LOG_LEVEL_KEY, "DEBUG");
ClientConfig config = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .logRequests(true)
        .logRequestStatus(true)
        .build();

TargetClient targetClient = TargetClient.create(config);
```

Dovresti vedere le richieste, le risposte e i tempi di risposta stampati nella console.
