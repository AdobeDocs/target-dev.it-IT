---
keywords: implementa, implementa, configura, imposta, parametro di pagina, tomcat, codifica url, attributo di profilo nella pagina, parametro mbox, attributi di profilo nella pagina, attributo di profilo script, API di aggiornamento del profilo bulk, API di aggiornamento di file singolo, attributi del cliente, implement5, implement6, implement7, implement8, implement9, implementing0, implementing1, implementing2, implementing3, implementing4, implementing5, fornitori di dati, provider di dati, provider di dati
description: Inserire dati in [!DNL Target] (parametri di pagina, attributi di profilo, attributi di profilo script, fornitori di dati, API di aggiornamento di profilo singolo e in blocco, attributi del cliente).
title: Come posso inserire dati in Target?
feature: Implementation
exl-id: a54e306a-ea8e-4d3f-bc5d-b5895b6b9a84
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 52%

---

# Panoramica dei metodi

Informazioni sui diversi metodi utilizzabili per immettere i dati in Adobe Target.

I metodi disponibili includono:

| Metodo | Dettagli |
| --- | --- |
| [Parametri di pagina](page-parameters.md)<br />(detti anche &quot;parametri mbox&quot;) | I parametri di pagina sono coppie di nome e valore passate direttamente tramite codice di pagina che non sono memorizzati nel profilo del visitatore per un utilizzo futuro.<br />I parametri di pagina sono utili per inviare i dati di pagina a [!DNL Target] che non deve essere memorizzato con il profilo del visitatore per utilizzi futuri di targeting. Questi valori vengono invece utilizzati per descrivere la pagina o l&#39;azione che l&#39;utente ha assunto nella pagina specifica. |
| [Attributi di profilo nella pagina](in-page-profile-attributes.md)<br />(detti anche &quot;attributi di profilo in-mbox&quot;) | Gli attributi di profilo nella pagina sono coppie di nome e valore passate direttamente tramite il codice della pagina archiviato nel profilo del visitatore per un utilizzo futuro.<br />Gli attributi di profilo nella pagina consentono l&#39;archiviazione dei dati specifici dell&#39;utente nel profilo di Target per targeting e segmentazione in un secondo momento. |
| [Attributi di profilo script](script-profile-attributes.md) | Gli attributi del profilo di script sono coppie nome/valore definite nella [!DNL Target] soluzione. Il valore è determinato dall&#39;esecuzione di un frammento JavaScript sul server di destinazione per ogni chiamata del server.<br />Gli utenti scrivono frammenti di codice di piccole dimensioni che vengono eseguiti per chiamata mbox e prima che un visitatore venga valutato per l&#39;appartenenza al pubblico e all&#39;attività. |
| [Fornitori di dati](data-providers.md) | I fornitori di dati ti consentono di trasmettere facilmente i dati da terze parti a Target. |
| [API di aggiornamento del profilo bulk](bulk-profile-update-api.md) | Tramite API, invia un file .csv a [!DNL Target] con aggiornamenti del profilo visitatore per molti visitatori. Ogni profilo visitatore può essere aggiornato con più attributi di profilo nella pagina in una chiamata. |
| [Aggiornamento di singolo profilo API](single-profile-update-api.md) | Quasi identico all&#39;API di aggiornamento del profilo bulk, ma un solo profilo visitatore alla volta viene aggiornato, in linea con la chiamata API anziché con un file .csv. |
| [Attributi del cliente](customer-attributes.md) | Gli attributi dei clienti consentono di caricare i dati del profilo visitatori tramite FTP nell&#39;Experience Cloud. Una volta caricati, puoi utilizzarli in Adobe Analytics e Adobe Target. |
