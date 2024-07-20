---
title: Aggiornare profili
description: Scopri come utilizzare le API del profilo di Adobe Target per inviare i dati dei visitatori a  [!DNL Target].
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
source-git-commit: 289299a52e5611c0da341f313aa4a447fcf3666a
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 1%

---

# Aggiornare profili

Un profilo utente contiene informazioni demografiche e comportamentali relative a un visitatore di una pagina web, ad esempio età, genere, prodotti acquistati, ultima visita e così via. [!DNL Adobe Target] utilizza queste informazioni per personalizzare il contenuto che fornisce a ogni visitatore.

Le informazioni di profilo per ciascun visitatore vengono memorizzate nei cookie o nelle app di terze parti.

Se la pagina Web implementa il codice [!DNL Target] ([at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) o [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)), le informazioni sul profilo dai cookie vengono passate a [!DNL Target] utilizzando i parametri del profilo. [!DNL Target] identifica ogni visitatore in modo univoco tramite un `pcID` generato nei cookie del visitatore. Tuttavia, puoi trasmettere parametri di profilo da un&#39;app esterna tramite chiamate mbox utilizzando `mbox3rdPartyIds`.

Utilizza le API di profilo [!DNL Adobe Target] quando disponi di dati di profilo sui visitatori da inviare a [!DNL Target] che non puoi o non desideri inviare come parte dell&#39;integrazione basata su pagina con [!DNL Target]. Potrebbero essere dati provenienti da un sistema CRM (Customer Relationship Management) o POS (Point of Sale) non disponibile nella pagina. Oppure questi dati potrebbero essere di natura più delicata e non avrebbe senso trasmetterli sulla pagina.

Esistono due modi per aggiornare i profili tramite API:

* [Aggiornamento di singolo profilo API](/help/dev/administer/profile-api/profile-single-api.md)
* [Aggiornamento del profilo bulk tramite batch](/help/dev/administer/profile-api/profile-bulk-api.md)
