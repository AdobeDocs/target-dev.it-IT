---
title: Panoramica delle API del profilo di Adobe Target
description: Scopri come utilizzare le API di profilo di Adobe Target per inviare dati dei visitatori a [!DNL Target].
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
source-git-commit: af9db32d59bdf32f2b9fade267922803250377dd
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 1%

---

# [!DNL Adobe Target Profile APIs overview]

Un profilo utente contiene informazioni demografiche e comportamentali relative a un visitatore di una pagina web, ad esempio età, genere, prodotti acquistati, ultima visita e così via. [!DNL Adobe Target] utilizza queste informazioni per personalizzare il contenuto che distribuisce a ogni visitatore.

Le informazioni di profilo per ciascun visitatore vengono memorizzate nei cookie o nelle app di terze parti.

Se la pagina web implementa il codice Target ([at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) o [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)), le informazioni del profilo dai cookie vengono trasmesse a [!DNL Target] utilizzando i parametri di profilo. [!DNL Target] identifica ogni visitatore in modo univoco tramite una `pcID` che genera i cookie del visitatore. Tuttavia, puoi trasmettere i parametri di profilo da un’app esterna tramite chiamate mbox utilizzando `mbox3rdPartyIds`.

Utilizza il [!DNL Adobe Target] API di profilo quando disponi di dati di profilo sui visitatori da inviare a [!DNL Target] che non puoi o non desideri inviare come parte dell&#39;integrazione basata su pagina con [!DNL Target]. Potrebbe trattarsi di dati provenienti da un sistema CRM (Customer Relationship Management) o POS (Point of Sale) che non sono disponibili nella pagina, o di dati di natura più delicata che non ha senso trasmettere sulla pagina.

Esistono due modi per aggiornare i profili tramite API:

* [Aggiornamento di singolo profilo API](/help/dev/administer/profile-api/profile-single-api.md)
* [Aggiornamento del profilo bulk tramite batch](/help/dev/administer/profile-api/profile-bulk-api.md)
