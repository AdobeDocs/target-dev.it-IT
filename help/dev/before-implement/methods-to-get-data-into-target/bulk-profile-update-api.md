---
keywords: implementazione, implementazione, configurazione, aggiornamento in blocco delle api dei profili
description: Ottieni dati in [!DNL Target] utilizzando [!UICONTROL Bulk Profile Update API].
title: Come posso inserire dati in [!DNL Target] utilizzando [!UICONTROL Bulk Profile Update API]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 7%

---

# API di aggiornamento del profilo bulk

[!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] consente di aggiornare i profili utente per più visitatori a un sito Web in blocco utilizzando un file batch.

Utilizzando [!UICONTROL Bulk Profile Update API], puoi inviare in modo comodo dati dettagliati del profilo visitatore sotto forma di parametri di profilo per molti utenti a [!DNL Target] da qualsiasi origine esterna. Le fonti esterne possono includere sistemi CRM (Customer Relationship Management) o POS (Point of Sale), che di solito non sono disponibili su una pagina web.

Contrasta [!UICONTROL Bulk Profile Update API] con [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md).

## [!UICONTROL Customer attributes] rispetto a [!UICONTROL Bulk Profile Update API]

Questa opzione è simile a [[!UICONTROL customer attributes]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md) con alcune differenze:

* [!UICONTROL Customer attributes] utilizza un caricamento FTP. [!UICONTROL Target Bulk Profile Update API] utilizza un&#39;API HTTP POST.
* I dati di [!UICONTROL Customer attribute] possono essere condivisi con [!DNL Analytics]. [!UICONTROL Bulk Profile Update] è utilizzabile solo in [!DNL Target].
* Il supporto di [!UICONTROL Customer attributes] per la creazione di un profilo per un utente [!DNL Target] non è stato ancora visualizzato.
   * [!UICONTROL Bulk Profile Update API] v2: non è necessario specificare tutti i valori dei parametri per ogni `pcId`. I profili creati per qualsiasi `pcId` o `mbox3rdPartyId` non trovato in [!DNL Target].
   * [!UICONTROL Bulk Profile Update API] v1: [!UICONTROL Bulk Profile Update API] aggiorna solo i profili [!DNL Target] esistenti. Se utilizzi la versione 1, i profili non vengono creati per `pcIds` o `mbox3rdPartyIds` mancanti.
* [!UICONTROL Customer attributes] richiedono l&#39;utilizzo di [!UICONTROL Experience Cloud ID] (ECID) e l&#39;utilizzo di un ID di origine, ad esempio l&#39;ID CRM o l&#39;ID fedeltà.
* [!UICONTROL Bulk Profile Update API] richiede l&#39;ID TNT o `mbox3rdPartyId`.
* Non puoi inviare i seguenti caratteri in `mbox3rdPartyID`: segno più (+) e barra (/).

## Risorse

Per ulteriori informazioni, consulta:

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)