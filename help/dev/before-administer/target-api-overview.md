---
title: Panoramica API di Adobe Target
description: Panoramica delle diverse API di Adobe Target, tra cui API di consegna, API di reporting, API di amministrazione, API di profilo, API di consigli e collegamenti alle raccolte postman.
exl-id: bf886103-36af-4061-b8be-2fe645f45ff3
feature: APIs/SDKs
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '440'
ht-degree: 1%

---

# Panoramica API di Target

Questo articolo descrive le diverse API di Target in generale, prima di concentrarsi sui requisiti specifici delle API amministratore e profilo. Se desideri amministrare Target tramite l’interfaccia utente, consulta la sezione [sezione di somministrazione del *Guida utente di Adobe Target Business*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=en).

## Tipi di API

Le API di Adobe Target sono una raccolta di API che alimentano i prodotti Adobe Target, come Adobe Recommendations. Queste API consentono di creare interfacce utente ricche di dati da utilizzare per manipolare e integrare i dati.

Le API di Adobe Target possono essere raggruppate per tipo: Amministratore, Profilo, Consegna e Reporting.

>[!NOTE]
>
>Le API amministratore e le API profilo sono spesso indicate collettivamente (&quot;API amministratore e API profilo&quot;), ma è possibile fare riferimento a esse separatamente (&quot;API amministratore&quot; e &quot;API profilo&quot;). L’API Recommendations è un’implementazione specifica di un’API amministratore di Target.

| Tipo di API | Cosa ti consente di fare | Collegamento di download | Altri collegamenti utili |
| --- | --- | --- |--- |
| [Amministrazione](../administer/admin-api/admin-api-overview-new.md) | Crea, modifica ed elimina attività, tipi di pubblico, offerte e altri oggetti (tra cui entità Recommendations, criteri, progettazioni e così via). Le API di Recommendations sono un tipo di API amministratore.) | <UL><li>[Raccolta Postman API amministratore di Target](https://developers.adobetarget.com/api/#admin-postman-collection)</li><li>[Raccolta Postman API Recommendations](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman)</li></UL> | [Utilizzare le API di Recommendations](../before-administer/recs-api/overview.md) |
| Profilo | Recupera e modifica i profili utente memorizzati in Adobe Target. | [Raccolta Postman API profilo di Target](https://developers.adobetarget.com/api/#profiles) |  |
| [Consegna](../implement/delivery-api/overview.md) | Recupera contenuti ottimizzati e personalizzati da Target per la consegna a un utente finale. | [Raccolta Postman API di consegna di Target](/help/dev/before-implement/delivery-api-overview/getting-started.md#postman) |  |
| [Generazione di rapporti](../administer/admin-api/admin-api-overview-new.md) | Esporta i risultati dell’attività e altri risultati di reporting. | Le API di reporting sono incluse in [Raccolta Postman API amministratore di Target](https://developers.adobetarget.com/api/#admin-postman-collection). |  |
| [Modelli](../administer/models-api/models-api-overview.md) | Gestisci l’elenco di funzioni che desideri escludere da Target dai suoi modelli di apprendimento automatico (il &quot;inserisco nell&#39;elenco Bloccati di apprendimento automatico&quot;). Models API è un tipo di API amministratore, ma viene elencato qui separatamente a causa delle sue operazioni univoche su oggetti (inserisce nell&#39;elenco Bloccati di) non accessibili tramite l’interfaccia utente. |  |  |

## Differenze tra API

Esistono importanti distinzioni tra le API amministratore di Target (incluse le API Recommendations) e le API di consegna di Target:

* Le API amministratore ti consentono di configurare vari aspetti di Target che puoi anche configurare nell’interfaccia utente di Target. Fa eccezione Models API, che consente di configurare aspetti di Target non disponibili nell’interfaccia utente. Indipendentemente, **tutte le API amministratore richiedono l’autenticazione.**

* Le API di consegna ti consentono di recuperare il contenuto. Le API di consegna non richiedono l’autenticazione.

Per utilizzare le API amministratore di Target, devi configurare l’autenticazione utilizzando [Console Adobe Developer](https://developer.adobe.com/console/home). Per ulteriori informazioni, consulta [Come configurare l’autenticazione](../before-administer/configure-authentication.md).
