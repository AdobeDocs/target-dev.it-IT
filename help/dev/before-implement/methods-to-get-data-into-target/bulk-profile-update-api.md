---
keywords: implementazione, implementazione, configurazione, aggiornamento in blocco delle api dei profili
description: Ottieni dati in [!DNL Target] utilizzando l'API di aggiornamento del profilo in blocco .
title: Come posso inserire dati in [!DNL Target] utilizzando l'[!UICONTROL API di aggiornamento del profilo bulk]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
TQID: https://experienceleague.adobe.com/vBcIsBR6wwYr7MbRh7UJvt71ULDEq6iXVZ5spZlif-0
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
source-wordcount: 284
ht-degree: 5%

---

# API di aggiornamento del profilo bulk

L&#39;API [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] consente di aggiornare i profili utente per più visitatori in un sito Web in blocco utilizzando un file batch.

Utilizzando l&#39;[!UICONTROL API di aggiornamento del profilo bulk], puoi inviare dati dettagliati del profilo del visitatore sotto forma di parametri di profilo per molti utenti a [!DNL Target] da qualsiasi origine esterna. Le fonti esterne possono includere sistemi CRM (Customer Relationship Management) o POS (Point of Sale), che di solito non sono disponibili su una pagina web.

Contrasto tra l&#39;[!UICONTROL API di aggiornamento del profilo bulk] e [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md).

## [!UICONTROL Attributi del cliente] rispetto all&#39;[!UICONTROL API di aggiornamento del profilo bulk]

Questa opzione è simile a [[!UICONTROL attributi cliente]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md) con alcune differenze:

* [!UICONTROL Gli attributi del cliente] utilizzano un caricamento FTP. L&#39;API [!UICONTROL Target Bulk Profile Update] utilizza un&#39;API HTTP POST.
* [!UICONTROL I dati dell&#39;attributo del cliente] possono essere condivisi con [!DNL Analytics]. L&#39;[!UICONTROL aggiornamento del profilo bulk] è utilizzabile solo in [!DNL Target].
* [!UICONTROL Gli attributi del cliente] supportano la creazione di un profilo per un utente [!DNL Target].
   * [!UICONTROL API di aggiornamento del profilo bulk] v2: non è necessario specificare tutti i valori dei parametri per ogni `pcId`. I profili creati per qualsiasi `pcId` o `mbox3rdPartyId` non trovato in [!DNL Target].
   * [!UICONTROL API di aggiornamento del profilo bulk] v1: [!UICONTROL API di aggiornamento del profilo bulk] aggiorna solo i profili [!DNL Target] esistenti. Se utilizzi la versione 1, i profili non vengono creati per `pcIds` o `mbox3rdPartyIds` mancanti.
* [!UICONTROL Gli attributi del cliente] richiedono l&#39;utilizzo di [!UICONTROL Experience Cloud ID] (ECID) e l&#39;utilizzo di un ID di origine, ad esempio l&#39;ID CRM o l&#39;ID fedeltà.
* L&#39;API [!UICONTROL Bulk Profile Update] richiede l&#39;ID TNT o `mbox3rdPartyId`.
* Non puoi inviare i seguenti caratteri in `mbox3rdPartyID`: segno più (+) e barra (/).

## Risorse

Per ulteriori informazioni, consulta:

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)