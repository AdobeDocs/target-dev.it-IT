---
keywords: implementare, implementare, configurare, configurare, attributi cliente
description: Ottieni dati in [!DNL Target] utilizzando gli attributi del cliente.
title: Come posso inserire dati in [!DNL Target] utilizzando gli attributi del cliente?
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
TQID: https://experienceleague.adobe.com/bzK915y7fvjfZjTkSK2QWHDzmIN9SdAQiEguiUlc-r8
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 216
ht-degree: 13%

---

# Attributi del cliente

Gli attributi del cliente ti consentono di caricare i dati del profilo del visitatore tramite FTP in [!DNL Adobe Experience Cloud]. Una volta caricati, utilizzare i dati in [!DNL Adobe Analytics] e [!DNL Adobe Target].

I clienti Target Standard possono applicare cinque attributi, [!DNL Target Premium] possono applicare 200 attributi.

## Formato

Un file .csv con [!DNL Experience Cloud] ID (ECID) e coppie nome attributo/valore viene caricato tramite FTP o manualmente nell&#39;interfaccia utente di Experience Cloud.

## Casi d’uso di esempio

Il tuo CRM o altro sistema interno memorizza informazioni preziose che desideri condividere con [!DNL Adobe Experience Cloud], inclusi [!DNL Target] e [!DNL Analytics].

## Vantaggi del metodo

Il caricamento dei dati del cliente crea una voce di profilo per quel visitatore in Target, anche se [!DNL Target] non ha ancora visto il visitatore.

Gli stessi dati sono automaticamente disponibili in [!DNL Target] e [!DNL Analytics].

FTP può essere un metodo di implementazione più semplice rispetto all&#39;API.

## Avvertenze

I clienti Target Standard possono applicare cinque attributi, [!DNL Target Premium] possono applicare 200 attributi

Può solo aggiornare i valori tramite gli attributi del cliente, non sulla pagina.

Richiede l&#39;implementazione di Experience Cloud ID (ECID).

## Esempi di codice

I dettagli sono disponibili in [Creare un&#39;origine attributo del cliente e caricare il file di dati](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=it).

### Collegamenti alle informazioni rilevanti

[Crea un&#39;origine attributo del cliente e carica il file di dati](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=it).
