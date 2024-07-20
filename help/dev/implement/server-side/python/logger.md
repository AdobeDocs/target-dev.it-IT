---
title: Inizializza l'SDK di Python  [!DNL Adobe Target]  per registrare le richieste
description: Scopri come registrare le richieste nell’SDK di Python [!DNL Adobe Target] .
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 3%

---

# Logger (Python)

## Descrizione

Quando [viene inizializzato l&#39;SDK](initialize-sdk.md), l&#39;oggetto `options["logger"]` è un oggetto facoltativo. Per impostazione predefinita, verrà creato un logger a livello INFO nello spazio dei nomi `adobe.target`. Tuttavia, per personalizzare il livello di registro o eseguire il debug in modo efficace quando si verifica un problema, è possibile fornire un oggetto `logger` durante l&#39;inizializzazione dell&#39;SDK.

Per l&#39;oggetto `logger` è previsto un metodo `debug()` e `error()`. Se viene fornito un logger appropriato, verranno registrate [!DNL Target] richieste e risposte.

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
