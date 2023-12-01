---
keywords: implementazione, implementazione, configurazione, aggiornamento in blocco delle api dei profili
description: Inserire dati in [!DNL Target] utilizzando [!UICONTROL API di aggiornamento del profilo bulk].
title: Come posso inserire dati in [!DNL Target] Utilizzo di [!UICONTROL API di aggiornamento del profilo bulk]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 5%

---

# API di aggiornamento del profilo bulk

Il [!DNL Adobe Target] [!UICONTROL API di aggiornamento del profilo bulk] consente di aggiornare in blocco i profili utente di più visitatori a un sito web utilizzando un file batch.

Utilizzo di [!UICONTROL API di aggiornamento del profilo bulk], puoi inviare in modo comodo dati dettagliati del profilo del visitatore sotto forma di parametri di profilo per molti utenti a [!DNL Target] da qualsiasi origine esterna. Le fonti esterne possono includere sistemi CRM (Customer Relationship Management) o POS (Point of Sale), che di solito non sono disponibili su una pagina web.

Contrasto [!UICONTROL API di aggiornamento del profilo bulk] con [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md).

## [!UICONTROL Attributi del cliente] rispetto al [!UICONTROL API di aggiornamento del profilo bulk]

Questa opzione è simile a [[!UICONTROL attributi del cliente]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md) con alcune differenze:

* [!UICONTROL Attributi del cliente] utilizzare un caricamento FTP. Il [!UICONTROL API di aggiornamento del profilo bulk di Target] utilizza un’API HTTP POST.
* [!UICONTROL Attributo cliente] i dati possono essere condivisi con [!DNL Analytics]. Il [!UICONTROL Aggiornamento del profilo bulk] è utilizzabile solo in [!DNL Target].
* [!UICONTROL Attributi del cliente] supporto per la creazione di un profilo per un utente [!DNL Target] non ha ancora visto.
   * [!UICONTROL API di aggiornamento del profilo bulk] v2: Non è necessario specificare tutti i valori dei parametri per ciascuno `pcId`. I profili vengono creati per qualsiasi `pcId` o `mbox3rdPartyId` che non si trova in [!DNL Target].
   * [!UICONTROL API di aggiornamento del profilo bulk] v1: The [!UICONTROL API di aggiornamento del profilo bulk] aggiornamenti esistenti [!DNL Target] solo profili. Se utilizzi la versione 1, i profili non vengono creati per mancanti `pcIds` o `mbox3rdPartyIds`.
* [!UICONTROL Attributi del cliente] richiedere l&#39;utilizzo di [!UICONTROL ID EXPERIENCE CLOUD] (ECID) e l’utilizzo di un ID sorgente, ad esempio l’ID del sistema di gestione delle relazioni con i clienti o l’ID fedeltà.
* Il [!UICONTROL API di aggiornamento del profilo bulk] richiede l&#39;ID TNT o `mbox3rdPartyId`.
* Non puoi inviare i seguenti caratteri in `mbox3rdPartyID`: segno più (+) e barra (/).

## Risorse

Per ulteriori informazioni, consulta:

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)