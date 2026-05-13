---
title: Panoramica API di Adobe Target
description: Panoramica delle diverse API di Adobe Target, tra cui API di consegna, API di reporting, API di amministrazione, API di profilo, API di consigli e collegamenti alle raccolte postman.
exl-id: bf886103-36af-4061-b8be-2fe645f45ff3
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/GbrWhrZxH-sTtpxotpJGbr-sHuIXrX7rZFQhju76-vM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 448
ht-degree: 0%

---

# Panoramica API di Target

Questo articolo descrive le diverse API di Target in generale, prima di concentrarsi sui requisiti specifici delle API amministratore e profilo. Se desideri amministrare Target tramite l&#39;interfaccia utente, consulta la [sezione amministrazione della *Guida utente di Adobe Target Business*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=it).

## Tipi di API

Le API di Adobe Target sono una raccolta di API che alimentano i prodotti Adobe Target, come Adobe Recommendations. Queste API consentono di creare interfacce utente ricche di dati da utilizzare per manipolare e integrare i dati.

Le API di Adobe Target possono essere raggruppate per tipo: Amministratore, Profilo, Consegna e Reporting.

>[!NOTE]
>
>Le API amministratore e le API profilo sono spesso indicate collettivamente (&quot;API amministratore e API profilo&quot;), ma è possibile fare riferimento a esse separatamente (&quot;API amministratore&quot; e &quot;API profilo&quot;). L’API Recommendations (Consigli) è un’implementazione specifica di un’API amministratore di Target.

| Tipo di API | Cosa ti consente di fare | Collegamento di download | Altri collegamenti utili |
| --- | --- | --- |--- |
| [Amministratore](../administer/admin-api/admin-api-overview-new.md) | Crea, modifica ed elimina attività, tipi di pubblico, offerte e altri oggetti (tra cui entità, criteri, progettazioni e così via). Le API Recommendations sono un tipo di API amministratore.) | <UL><li>[Raccolta Postman API amministratore di Target](https://developers.adobetarget.com/api/#admin-postman-collection)</li><li>[Raccolta Postman API Recommendations](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman)</li></UL> | [Utilizzare le API Recommendations](../before-administer/recs-api/overview.md) |
| Profilo | Recupera e modifica i profili utente memorizzati in Adobe Target. | [Raccolta Postman API profilo di Target](https://developers.adobetarget.com/api/#profiles) |  |
| [Consegna](../implement/delivery-api/overview.md) | Recupera contenuti ottimizzati e personalizzati da Target per la consegna a un utente finale. | [Raccolta Postman API di consegna di Target](/help/dev/before-implement/delivery-api-overview/getting-started.md#postman) |  |
| [Generazione di rapporti](../administer/admin-api/admin-api-overview-new.md) | Esporta i risultati dell’attività e altri risultati di reporting. | Le API di reporting sono incluse nella [raccolta Postman dell&#39;API amministratore di Target](https://developers.adobetarget.com/api/#admin-postman-collection). |  |
| [Modelli](../administer/models-api/models-api-overview.md) | Gestisci l’elenco di funzioni che desideri escludere da Target dai suoi modelli di apprendimento automatico (il &quot;inserisco nell&#39;elenco Bloccati di apprendimento automatico&quot;). Models API è un tipo di API amministratore, ma viene elencato qui separatamente a causa delle sue operazioni univoche su oggetti (inserisce nell&#39;elenco Bloccati di) non accessibili tramite l’interfaccia utente. |  |  |

## Differenze tra API

Esistono importanti distinzioni tra le API amministratore di Target (incluse le API Recommendations) e le API di consegna di Target:

* Le API amministratore ti consentono di configurare vari aspetti di Target che puoi anche configurare nell’interfaccia utente di Target. Fa eccezione Models API, che consente di configurare aspetti di Target non disponibili nell’interfaccia utente. Indipendentemente da ciò, **tutte le API amministratore richiedono l&#39;autenticazione.**

* Le API di consegna ti consentono di recuperare il contenuto. Le API di consegna non richiedono l’autenticazione.

Per utilizzare le API dell&#39;amministratore di Target, è necessario configurare l&#39;autenticazione utilizzando [Adobe Developer Console](https://developer.adobe.com/console/home). Per ulteriori informazioni, vedere [Come configurare l&#39;autenticazione](../before-administer/configure-authentication.md).
