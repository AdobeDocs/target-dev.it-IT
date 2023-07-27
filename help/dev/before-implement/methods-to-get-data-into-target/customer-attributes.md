---
keywords: implementare, implementare, configurare, configurare, attributi cliente
description: Inserire dati in [!DNL Target] utilizzo degli attributi del cliente.
title: Come posso inserire dati in [!DNL Target] Utilizzare Attributi del cliente?
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 18%

---

# Attributi del cliente

Gli attributi del cliente ti consentono di caricare i dati del profilo del visitatore tramite FTP su [!DNL Adobe Experience Cloud]. Una volta caricati, puoi utilizzarli in [!DNL Adobe Analytics] e [!DNL Adobe Target].

I clienti di Target Standard possono applicare cinque attributi, [!DNL Target Premium] I clienti possono applicare 200 attributi.

## Formato

Un file .csv con [!DNL Experience Cloud] Le coppie ID (ECID) e nome attributo/valore vengono caricate tramite FTP o manualmente nell’interfaccia utente di Experience Cloud.

## Casi d’uso di esempio

Il tuo CRM o altro sistema interno memorizza informazioni preziose che desideri condividere con [!DNL Adobe Experience Cloud], tra cui [!DNL Target] e [!DNL Analytics].

## Vantaggi del metodo

Il caricamento dei dati dei clienti crea una voce di profilo per quel visitatore in Target, anche se [!DNL Target] deve ancora vedere il visitatore.

Gli stessi dati sono disponibili automaticamente in [!DNL Target] e [!DNL Analytics].

FTP può essere un metodo di implementazione più semplice rispetto all&#39;API.

## Avvertenze

I clienti di Target Standard possono applicare cinque attributi, [!DNL Target Premium] i clienti possono applicare 200 attributi

Può solo aggiornare i valori tramite gli attributi del cliente, non sulla pagina.

Richiede l&#39;implementazione di Experience Cloud ID (ECID).

## Esempi di codice

I dettagli sono disponibili in [Creare un&#39;origine attributo del cliente e caricare il file di dati](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html).

### Collegamenti alle informazioni rilevanti

[Crea un&#39;origine attributo del cliente e carica il file di dati](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html).
