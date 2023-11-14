---
title: Cos’è l’API di Adobe Recommendations?
description: Questa guida illustra agli sviluppatori le procedure pratiche per l’utilizzo delle API Recommendations di Adobe Target per configurare e gestire cataloghi Recommendations e criteri personalizzati, nonché per l’utilizzo dell’API di consegna per recuperare i contenuti dei consigli.
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 1%

---

# Panoramica API di Adobe Recommendations

Le API rilevanti per Recommendations includono [API amministratore](../../before-administer/target-api-overview.md) che ti consentono di:

* Gestione del catalogo di prodotti o contenuti consigliati
* Gestione di algoritmi e attività di Recommendations

Utilizzo di Target [API di consegna](../../implement/delivery-api/overview.md) con Recommendations è inoltre possibile:

* Recupera i consigli in oggetti JSON, HTML o XML per visualizzarli in web, dispositivi mobili, e-mail, Internet of Things (IOT) e altri canali.

## Descrizione

Questa guida relativa alle API di Recommendations illustra agli sviluppatori le procedure pratiche per l’utilizzo delle API di Recommendations per configurare e gestire cataloghi Recommendations e criteri personalizzati, nonché per l’utilizzo delle API di consegna per recuperare i contenuti dei consigli. Entro la fine, potrai:

* Configurare e gestire le entità tramite l’API Recommendations
* Configurare e gestire criteri personalizzati utilizzando l’API di Recommendations
* Scopri come utilizzare Recommendations con l’API di consegna per utilizzare i risultati dei consigli in dispositivi non HTML.

## Destinatari

Questa guida è destinata agli sviluppatori che utilizzano per la prima volta le API di Target o le API di Recommendations.

## Prerequisiti {#prerequisites}

Le API dell’amministratore di Target richiedono [Impostazione dell’autenticazione Adobe](../configure-authentication.md). Assicurati di averlo configurato prima di utilizzare l’API di Recommendations.

## Risorse

Prendi nota delle seguenti risorse, necessarie per comprendere questa guida e seguirla correttamente:

| Risorsa | Dettagli |
| --- | --- |
| Postman | Ottieni [app Postman](https://www.postman.com/downloads/) per il sistema operativo. Postman basic è gratuito con la creazione dell&#39;account. Anche se non è necessario per utilizzare le API di Adobe Target in generale, Postman semplifica i flussi di lavoro API e Adobe Target fornisce diverse raccolte Postman per aiutarle a eseguire le API e a imparare a utilizzarle. Il resto di questa guida presuppone una conoscenza operativa di Postman. Per assistenza, fai riferimento a [Documentazione di Postman](https://learning.getpostman.com/). |
| Riferimenti | Per il resto di questa guida si presume che le risorse seguenti siano familiari:<UL><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Documentazione API per l’amministrazione di Target e il profilo](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Documentazione API di Recommendations](https://developer.adobe.com/target/administer/recommendations-api/)</li></UL> |
