---
keywords: implementare, implementare, configurare, configurare, attributi cliente
description: Ottieni dati in [!DNL Target] utilizzando gli attributi del cliente.
title: Come posso inserire dati in [!DNL Target] utilizzando gli attributi del cliente?
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 14%

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
