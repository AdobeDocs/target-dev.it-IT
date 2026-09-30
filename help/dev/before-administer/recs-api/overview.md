---
title: Cos’è l’API di Adobe Recommendations?
description: Questa guida illustra agli sviluppatori le procedure pratiche per l’utilizzo delle API di Adobe Target Recommendations per configurare e gestire cataloghi e criteri personalizzati di Recommendations, nonché per l’utilizzo delle API di consegna per recuperare i contenuti di Recommendations.
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail:
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
TQID: 'https://experienceleague.adobe.com/-bWsxWNZK7LXp0VvKZmsZc68jXcit57v7Wki9hR3wH4'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: a19e8738-9679-599a-b83b-5f2f15f8e4d6
    internal-label: APIs/SDKs
  - id: dfc8a233-f2b5-4811-bf63-b4262aebc5a5
    internal-label: Administration and configuration
  - id: f69bc5f1-ebdb-4306-a281-f2e77daf734c
    internal-label: Activities and tests
subfeature_v2:
  - id: ed58f4a1-16eb-4c8c-b505-be9da766a9ec
    internal-label: Recommendations
  - id: fc9c2184-9102-403f-bd6c-0055021e4bea
    internal-label: Overview
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
    internal-label: Reporting
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 3%
---
# Panoramica API di Adobe Recommendations

Le API rilevanti per la funzione Consigli includono [API amministratore](../../before-administer/target-api-overview.md) che consentono di:

* Gestione del catalogo di prodotti o contenuti consigliati
* Gestire gli algoritmi e le attività di Recommendations

Utilizzando l&#39;API di consegna [di Target](../../implement/delivery-api/overview.md) con Recommendations, puoi anche:

* Recupera i consigli in oggetti JSON, HTML o XML in modo che possano essere visualizzati in web, dispositivi mobili, e-mail, Internet of Things (IOT) e altri canali.

## Descrizione

Questa guida relativa alle API di Recommendations illustra agli sviluppatori le procedure pratiche per l’utilizzo delle API di Recommendations per configurare e gestire cataloghi e criteri personalizzati di Recommendations, nonché per l’utilizzo delle API di consegna per recuperare i contenuti di Recommendations. Entro la fine, potrai:

* Configurare e gestire le entità tramite l’API Recommendations
* Configurare e gestire criteri personalizzati utilizzando l’API di Recommendations
* Scopri come utilizzare Recommendations con l’API di consegna per utilizzare i risultati dei consigli in dispositivi non HTML

## Pubblico

Questa guida è destinata agli sviluppatori che utilizzano per la prima volta le API di Target o le API di Consigli.

## Prerequisiti {#prerequisites}

Le API dell&#39;amministratore di Target richiedono [l&#39;installazione dell&#39;autenticazione Adobe](../configure-authentication.md). Assicurati di averlo configurato prima di utilizzare l’API di Recommendations.

## Risorse

Prendi nota delle seguenti risorse, necessarie per comprendere questa guida e seguirla correttamente:

| Risorsa | Dettagli |
| --- | --- |
| Postman | Scarica l&#39;[app Postman](https://www.postman.com/downloads/) per il tuo sistema operativo. Postman basic è gratuito con la creazione dell&#39;account. Anche se non è necessario per utilizzare le API di Adobe Target in generale, Postman semplifica i flussi di lavoro API e Adobe Target fornisce diverse raccolte Postman per aiutarle a eseguire le API e a imparare a utilizzarle. Il resto di questa guida presuppone una conoscenza operativa di Postman. Per assistenza, fare riferimento alla [documentazione di Postman](https://learning.getpostman.com/). |
| Riferimenti | Per il resto di questa guida si presume che le risorse seguenti siano familiari:<UL><li>[Github Adobe I/O](https://github.com/adobeio)</li><li>[Documentazione API per l&#39;amministrazione di Target e il profilo](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Documentazione API per la funzione Consigli](https://developer.adobe.com/target/administer/recommendations-api/)</li></UL> |
