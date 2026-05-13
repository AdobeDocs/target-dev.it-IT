---
keywords: implementa, implementa, configura, imposta, parametro di pagina, tomcat, codifica url, attributo di profilo nella pagina, parametro mbox, attributi di profilo nella pagina, attributo di profilo script, API di aggiornamento del profilo bulk, API di aggiornamento di file singolo, attributi del cliente, implement5, implement6, implement7, implement8, implement9, implementing0, implementing1, implementing2, implementing3, implementing4, implementing5, fornitori di dati, provider di dati, provider di dati
description: Ottieni dati in  [!DNL Target] (parametri di pagina, attributi di profilo, attributi di profilo script, fornitori di dati, API di aggiornamento di profilo singolo e in blocco, attributi del cliente).
title: Come posso inserire dati in Target?
feature: Implementation
exl-id: a54e306a-ea8e-4d3f-bc5d-b5895b6b9a84
TQID: https://experienceleague.adobe.com/pmlPWRHb9tnrdSFm7s5OZ-RRsJJOxw-ntBY5AeswIcM
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 377
ht-degree: 27%

---

# Panoramica dei metodi

Informazioni sui diversi metodi utilizzabili per immettere i dati in Adobe Target.

I metodi disponibili includono:

| Metodo | Dettagli |
| --- | --- |
| [Parametri pagina](page-parameters.md)<br />(detti anche &quot;parametri mbox&quot;) | I parametri di pagina sono coppie di nome e valore passate direttamente tramite codice di pagina che non sono memorizzati nel profilo del visitatore per un utilizzo futuro.<br />I parametri di pagina sono utili per inviare dati di pagina a [!DNL Target] che non devono essere memorizzati con il profilo del visitatore per utilizzi futuri di targeting. Questi valori vengono invece utilizzati per descrivere la pagina o l&#39;azione che l&#39;utente ha assunto nella pagina specifica. |
| [Attributi di profilo nella pagina](in-page-profile-attributes.md)<br />(detti anche &quot;attributi di profilo nella mbox&quot;) | Gli attributi del profilo nella pagina sono coppie nome/valore passate direttamente attraverso il codice della pagina e memorizzate nel profilo del visitatore per utilizzi futuri.<br />Gli attributi del profilo nella pagina consentono di memorizzare dati specifici dell&#39;utente nel profilo di Target per un targeting e una segmentazione successivi. |
| [Attributi di profilo script](script-profile-attributes.md) | Gli attributi del profilo di script sono coppie nome/valore definite nella soluzione [!DNL Target]. Il valore è determinato dall&#39;esecuzione di un frammento JavaScript sul server di destinazione per ogni chiamata del server.<br />Gli utenti scrivono piccoli snippet di codice da eseguire per chiamata mbox e prima che un visitatore venga valutato per l&#39;appartenenza a un pubblico e a un&#39;attività. |
| [Fornitori di dati](data-providers.md) | I fornitori di dati ti consentono di trasmettere facilmente i dati da terze parti a Target. |
| [API di aggiornamento del profilo bulk](bulk-profile-update-api.md) | Tramite API, invia un file .csv a [!DNL Target] con gli aggiornamenti del profilo del visitatore per molti visitatori. Ogni profilo visitatore può essere aggiornato con più attributi di profilo nella pagina in una chiamata. |
| [Aggiornamento di singolo profilo API](single-profile-update-api.md) | Quasi identico all&#39;API di aggiornamento del profilo bulk, ma un profilo visitatore viene aggiornato alla volta, in linea nella chiamata API invece di con un file .csv. |
| [Attributi del cliente](customer-attributes.md) | Gli attributi dei clienti consentono di caricare i dati del profilo visitatori tramite FTP nell&#39;Experience Cloud. Una volta caricati, puoi utilizzarli in Adobe Analytics e Adobe Target. |
