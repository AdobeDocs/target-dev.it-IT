---
title: Inizializzare [!DNL Adobe Target] SDK Java per registrare le richieste
description: Scopri come registrare le richieste in [!DNL Adobe Target] SDK Java.
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 4%

---

# Logger (Java)

## Descrizione

Quando [inizializzazione dell’SDK](initialize-sdk.md), sono disponibili diverse opzioni per `ClientConfig` , che può essere impostato per registrare le richieste.

| Opzione | Descrizione |
| --- | --- |
| `logRequests` | Registra l’intero corpo della richiesta e il corpo della risposta. |
| `logRequestStatus` | Registra l’URL della richiesta, lo stato e il tempo di risposta. |

[!DNL Target] L’SDK Java utilizza `slf4j` registrazione. Devi fornire la tua implementazione di logger, ad esempio `java.util.logging`, `logback`, e `log4j`. Fai riferimento a [http://www.slf4j.org/manual.html](http://www.slf4j.org/manual.html) per ulteriori informazioni. Tutti i registri verranno stampati in `debug`.

## Esempio

Aggiungi il `slf4j` dipendenza.

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

Abilita `DEBUG` registra in base all’implementazione e contrassegna i flag di registrazione delle richieste.

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
