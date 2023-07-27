---
title: Inizializzare [!DNL Adobe Target] SDK Python per registrare le richieste
description: Scopri come registrare le richieste in [!DNL Adobe Target] SDK Python.
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 3%

---

# Logger (Python)

## Descrizione

Quando [inizializzazione dell’SDK](initialize-sdk.md), il `options["logger"]` object è un oggetto facoltativo. Per impostazione predefinita, viene creato un logger a livello INFO sotto il `adobe.target` spazio dei nomi. Tuttavia, per personalizzare il livello di registro o eseguire il debug in modo efficace quando si verifica un problema, è necessario `logger` durante l&#39;inizializzazione dell&#39;SDK.

Il `logger` l&#39;oggetto deve avere `debug()` e un `error()` metodo. Quando viene fornito un logger appropriato, [!DNL Target] Le richieste e le risposte verranno registrate.

## Esempio

### Python

```python {line-numbers="true"}
logger = logging.getLogger("org.logger")
logger.setLevel(logging.DEBUG)

client_options = {
  "client": "acmeclient",
  "organization_id": "1234567890@AdobeOrg",
  "logger": logger
}
target_client = TargetClient.create(client_options)
```

Dovresti vedere le richieste e le risposte stampate nella console.
