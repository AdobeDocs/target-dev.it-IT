---
title: Aggiornare profili
description: Scopri come utilizzare le API del profilo di Adobe Target per inviare i dati dei visitatori a  [!DNL Target].
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/jclCuF4pe3JwAN-2RhQ9NfA5KtEfKUuawlmrE3aS0bQ
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 217
ht-degree: 1%

---

# Aggiornare profili

Un profilo utente contiene informazioni demografiche e comportamentali relative a un visitatore di una pagina web, ad esempio età, genere, prodotti acquistati, ultima visita e così via. [!DNL Adobe Target] utilizza queste informazioni per personalizzare il contenuto che fornisce a ogni visitatore.

Le informazioni di profilo per ciascun visitatore vengono memorizzate nei cookie o nelle app di terze parti.

Se la pagina Web implementa il codice [!DNL Target] ([at.js](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md) o [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)), le informazioni sul profilo dei cookie vengono passate a [!DNL Target] utilizzando i parametri del profilo. [!DNL Target] identifica ogni visitatore in modo univoco tramite un `pcID` generato nei cookie del visitatore. Tuttavia, puoi trasmettere parametri di profilo da un&#39;app esterna tramite chiamate mbox utilizzando `mbox3rdPartyIds`.

Utilizza le API di profilo [!DNL Adobe Target] quando disponi di dati di profilo sui visitatori da inviare a [!DNL Target] che non puoi o non desideri inviare come parte dell&#39;integrazione basata su pagina con [!DNL Target]. Potrebbero essere dati provenienti da un sistema CRM (Customer Relationship Management) o POS (Point of Sale) non disponibile nella pagina. Oppure questi dati potrebbero essere di natura più delicata e non avrebbe senso trasmetterli sulla pagina.

Esistono due modi per aggiornare i profili tramite API:

* [Aggiornamento di singolo profilo API](/help/dev/administer/profile-api/profile-single-api.md)
* [Aggiornamento del profilo bulk tramite batch](/help/dev/administer/profile-api/profile-bulk-api.md)
