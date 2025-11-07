---
title: Panoramica dell’API di amministrazione di Adobe Target
description: Panoramica di  [!DNL Adobe Target Admin API]
exl-id: 1168d376-c95b-4c5a-b7a2-c7815799a787
feature: APIs/SDKs
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '1305'
ht-degree: 2%

---

# Panoramica API dell’amministratore di Target

Questo articolo fornisce una panoramica delle informazioni di base necessarie per comprendere e utilizzare [!DNL Adobe Target Admin API] correttamente. Il contenuto seguente presuppone che tu sappia come [configurare l&#39;autenticazione](../configure-authentication.md) per [!DNL Adobe Target Admin API].

>[!NOTE]
>
>Se desideri amministrare [!DNL Target] tramite l&#39;interfaccia utente, consulta la [sezione amministrazione della *Guida di Adobe Target Business Practitioner*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=en).
>
>Le API amministratore e le API profilo sono spesso indicate collettivamente (&quot;API amministratore e API profilo&quot;), ma è possibile fare riferimento a esse separatamente (&quot;API amministratore&quot; e &quot;API profilo&quot;). L&#39;API Recommendations è un&#39;implementazione specifica di un&#39;API amministratore [!DNL Target].

## Prima di iniziare

In tutti gli esempi di codice forniti per le [API amministratore](../../administer/admin-api/admin-api-overview-new.md), sostituisci {tenant} con il valore tenant, `your-bearer-token` con il token di accesso generato con il tuo JWT e `your-api-key` con la tua chiave API di [Adobe Developer Console](https://developer.adobe.com/console/home). Per ulteriori informazioni su tenant e JWT, consulta l&#39;articolo su come [configurare l&#39;autenticazione](../configure-authentication.md) per le API amministratore di Adobe [!DNL Target].

## Controllo delle versioni

A tutte le API è associata una versione. È importante fornire la versione corretta dell’API che desideri utilizzare.

Se la richiesta contiene un payload (POST o PUT), l&#39;intestazione `Content-Type` della richiesta viene utilizzata per specificare la versione.

Se la richiesta non contiene un payload (GET, DELETE o OPTIONS), per specificare la versione viene utilizzata l&#39;intestazione `Accept`.

Se non viene fornita una versione, la chiamata sarà predefinita V1 (application/vnd.adobe.target.v1+json).

>[!NOTE]
>
>Se non viene specificata la versione corretta, ad esempio se si utilizza un payload V2 ma non si specifica l’intestazione Content-Type, l’API risponderà con un errore non supportato se l’API non è compatibile con le versioni precedenti.

Messaggio di errore per funzioni non supportate

```
{
    "httpStatus": 406,
    "requestId": "8752b736-cf71-4d81-86c3-94be2b5ae648",
    "requestTime": "2018-02-02T21:39:06.405Z",
    "errors": [
        {
            "errorCode": "Unsupported.Feature",
            "message": "Unsupported features detected"
        }
    ]
}
```

Raccolta Postman amministrazione

Postman è un’applicazione che semplifica l’attivazione delle chiamate API. Questa [raccolta Postman API amministratore di Target](https://developers.adobetarget.com/api/#admin-postman-collection) contiene tutte le chiamate API amministratore di Target che richiedono l&#39;autenticazione tramite attività, tipi di pubblico, offerte, report, mbox e ambienti

## Codici di risposta

Di seguito sono riportati i codici di risposta comuni per le API di amministrazione di Target.

| Stato | Significato | Descrizione |
| --- | --- | --- |
| 200 | [OK](https://www.rfc-editor.org/rfc/rfc7231#section-6.3.1) | OK |
| 400 | [Richiesta non valida](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.1) | Richiesta non valida. Probabilmente i dati forniti nella richiesta non sono validi. |
| 401 | [Non autorizzato](https://www.rfc-editor.org/rfc/rfc7235#section-3.1) | L&#39;utente non è autorizzato a eseguire questa operazione. |
| 403 | [Non consentito](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.3) | L’accesso a questa risorsa non è consentito. |
| 404 | [Non Trovato](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.4) | Impossibile trovare la risorsa di riferimento. |

## Attività

Un’attività ti consente di testare o personalizzare il contenuto per i tuoi utenti. Le attività possono essere di uno dei seguenti tipi:

* [A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [Targeting delle esperienze (XT)](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html)
* [Raccomandazioni](https://experienceleague.adobe.com/docs/target/using/activities/recommendations-activity.html)
* [Personalizzazione automatizzata](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [Test multivariato (MVT)](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)

## Aggiornamenti in blocco

È possibile eseguire più API amministratore come una singola richiesta batch.

### Esegui chiamate in batch

`POST /{tenant}/target/batch`

Sovrapponi più chiamate API ed eseguili in un singolo batch.

Il batch consente di trasmettere istruzioni per diverse operazioni in una singola richiesta HTTP. Puoi anche specificare le dipendenze tra operazioni correlate (descritte in una sezione seguente). TNT elaborerà ciascuna delle operazioni indipendenti (possibilmente in parallelo) ed elaborerà le operazioni dipendenti in sequenza. Una volta completate tutte le operazioni, verrà restituita una risposta consolidata e la connessione HTTP verrà chiusa.

L’API batch accetta un array di richieste HTTP logiche rappresentate come array JSON: ogni richiesta ha un metodo (corrispondente al metodo HTTP GET/PUT/POST/DELETE, ecc.), un URL relativo (la parte dell’URL dopo admin/rest/), un array di intestazioni facoltative (corrispondente alle intestazioni HTTP) e un corpo facoltativo (per le richieste POST e PUT). L’API Batch restituisce un array di risposte HTTP logiche rappresentate come array JSON: ogni risposta ha un codice di stato, un array di intestazioni facoltativo e un corpo facoltativo (che è una stringa codificata JSON). Per creare richieste in batch, crea un oggetto JSON che descrive ogni singola operazione da eseguire. Il numero massimo di operazioni consentite è 256 (da 0 a 255).

Specifica delle dipendenze tra le operazioni nella richiesta Per impostazione predefinita, le operazioni specificate nella richiesta API batch sono indipendenti: possono essere eseguite in ordine arbitrario sul server e un errore in un’operazione non influisce sull’esecuzione di altre operazioni.

Spesso, le operazioni nella richiesta dipendono - ad esempio, l’output di un’operazione può essere utilizzato nell’input dell’operazione successiva. Ad esempio, l’offerta creata in operationId=0 deve essere utilizzata nella creazione della campagna operationId=1.

Per collegare insieme due operazioni batch, specifica nell’operazione dipendente l’ID dell’operazione richiesta, ad esempio: &quot;dependentOnOperationId&quot; : 5. Inoltre, gli ID delle risorse create tramite richieste POST di operazioni batch possono essere utilizzati nelle operazioni dipendenti sia in &quot;relativeUrl&quot; che in &quot;body&quot;.

#### Autorizzazioni e limitazione

Per eseguire azioni API in batch, l’utente sottostante deve disporre almeno dei diritti di &quot;editor&quot; (per ogni singola operazione nel caso in cui siano necessari diritti aggiuntivi rispetto all’utente, la singola operazione non riuscirà). Le comuni strategie di limitazione vengono applicate alle azioni API batch come se ogni operazione fosse stata eseguita singolarmente.

L&#39;elaborazione batch termina quando tutte le operazioni sono state completate, un&#39;operazione potrebbe essere riuscita (codice stato 2xx), non riuscita (codice stato 4xx, 5xx) o ignorata perché un&#39;operazione di dipendenza non è riuscita o è stata ignorata.

#### Parametri oggetto di richiesta

| Attributo | Descrizione | Limiti | Predefinito |
| --- | --- | --- | --- |
| corpo | corpo dell&#39;operazione batch HTTP. verrà ignorato per tutte le azioni eccetto POST e PUT. può fare riferimento agli ID da azioni batch precedenti, ad esempio: &quot;offerId&quot;: &quot;{operationIdResponse:0}&quot;, &quot;segmentId&quot;: &quot;{operationIdResponse:1}&quot; | deve essere un JSON valido; nel caso di riferimento a un operationIdResponse, la risposta operationId a cui si fa riferimento deve essere un ID valido e il metodo su tale azione deve essere POST | oggetto vuoto {} |
| dependentOnOperationIds | elenco di ID vincolo che garantiranno che l&#39;operazione corrente verrà eseguita solo se le operazioni specificate sono state completate correttamente. Può essere utilizzato per ottenere il concatenamento delle operazioni. | sono consentite al massimo 255 operazioni; sono consentiti solo valori univoci; deve puntare a un operationId valido nell’array; non sono consentite dipendenze cicliche |  |
| intestazioni | array di intestazioni chiave-valore da inviare con una particolare operazione. Se l’autenticazione per l’API batch è stata eseguita tramite l’intestazione Autorizzazione, verrà copiata anche per le singole operazioni. | il numero massimo di intestazioni consentito nell’array è 50 | Content-Type: application/json |
| intestazioni->nome | nome intestazione | deve essere univoco tra gli altri nomi di intestazione. le intestazioni non distinguono tra maiuscole e minuscole per rfc, altrimenti i valori si sovrascriveranno a vicenda. |  |
| intestazioni->valore | valore intestazione | N/D | stringa vuota |
| metodo | Metodo HTTP da utilizzare. Opzioni disponibili: GET, POST, PUT, PATCH, DELETE | sono consentiti solo i metodi GET, POST, PUT, PATCH e DELETE |  |
| operationId | ID operazione utilizzato per identificare un’operazione tra altre operazioni per le risposte e i risultati di riferimento. | unico tra altre operazioni; valori da 0 a 255 |  |
| operazioni | elenco delle operazioni da eseguire in un batch. l&#39;ordine non è rilevante. | sono consentite al massimo 256 operazioni |  |
| relativeUrl | URL relativo per l’API rest di amministrazione, la parte successiva a &quot;/admin/rest/&quot;. Può contenere parametri della stringa di query come: &quot;/v2/campaigns?limit=10&amp;offset=10&quot;. può fare riferimento a URL con ID contenuti in azioni batch precedenti, ad esempio: &quot;/v1/offers/{operationIdResponse:0}&quot;. Se vengono inviati parametri di query, questi devono essere codificati nell’URL. | deve iniziare con / (essere relativo); sono supportate solo le nuove API JSON valide; in caso di URL relativo non valido verrà restituita una risposta 404 per un’operazione particolare; nel caso in cui si faccia riferimento a un operationIdResponse, la risposta operationId a cui si fa riferimento deve essere un ID valido e il metodo su tale azione deve essere POST |  |

#### Oggetto di richiesta di esempio

```
{
  "operations": [
    {
      "operationId": 1,
      "dependsOnOperationIds~": [0],
      "method": "POST",
      "relativeUrl": "/v1/offers",
      "headers~": [
        {
          "name": "Content-Type",
          "value": "application/json"
        }
      ],
      "body~": {
        "key": "value"
      }
    }
  ]
}
```

#### Parametri oggetto di risposta

| Parametro | Descrizione |
| --- | --- |
| operationId | ID operazione utilizzato per identificare un’operazione tra altre operazioni, lo stesso ID inviato nella richiesta POST. |
| ignorato | contrassegno boolen per contrassegnare se l&#39;operazione è stata eseguita o saltata. Sarà true se l&#39;operazione corrente dipende da un&#39;operazione non riuscita (ha restituito un valore statusCode diverso da 2xx). |
| statusCode | restituito, tutte le operazioni dipendenti verranno ignorate (non eseguite). |
| intestazioni | array di intestazioni chiave-valore da inviare come risposta per una particolare operazione. |
| intestazioni->nome | nome intestazione |
| intestazioni->valore | valore intestazione |
| corpo | corpo dell’operazione di risposta batch HTTP |

#### Oggetto risposta di esempio

```
{
  "results": [
    {
      "operationId": 1,
      "skipped~": false,
      "statusCode~": 200,
      "headers~": [
        {
          "name": "Content-Type",
          "value": "application/json; charset=UTF-8"
        }
      ],
      "body~": {
        "id": 5
      }
    }
  ]
}
```
