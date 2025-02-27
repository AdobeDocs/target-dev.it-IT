---
title: Inizializza  [!DNL Adobe Target] Java SDK per registrare le richieste
description: Scopri come registrare le richieste in  [!DNL Adobe Target] Java SDK.
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
source-git-commit: 526445fccee9b778b7ac0d7245338f235f11d333
workflow-type: tm+mt
source-wordcount: '118'
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
