---
keywords: implementazione, implementazione, configurazione, aggiornamento in blocco delle api dei profili
description: Inserire dati in [!DNL Target] utilizzando [!UICONTROL API di aggiornamento del profilo bulk].
title: Come posso inserire dati in [!DNL Target] Utilizzo di [!UICONTROL API di aggiornamento del profilo bulk]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: af9db32d59bdf32f2b9fade267922803250377dd
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 24%

---

# API di aggiornamento del profilo bulk

Tramite API, invia un file .csv a [!DNL Adobe Target] con aggiornamenti del profilo visitatore per molti visitatori. Ogni profilo visitatore può essere aggiornato con più attributi di profilo nella pagina in una chiamata.

Questa opzione è simile a [!UICONTROL attributi del cliente] con alcune differenze:

* [!UICONTROL Attributi del cliente] utilizzare un caricamento FTP. Il [!UICONTROL API di aggiornamento del profilo bulk di Target] utilizza un’API HTTP POST.
* [!UICONTROL Attributo cliente] i dati possono essere condivisi con [!DNL Analytics]. L&#39;aggiornamento del profilo bulk è utilizzabile solo in [!DNL Target].
* [!UICONTROL Attributi del cliente] supporto per la creazione di un profilo per un utente [!DNL Target] non ha ancora visto.
   * [!UICONTROL API di aggiornamento del profilo bulk] v2: Non è necessario specificare tutti i valori dei parametri per ciascuno `pcId`. I profili vengono creati per qualsiasi `pcId` o `mbox3rdPartyId` che non si trova in [!DNL Target].
   * [!UICONTROL API di aggiornamento del profilo bulk] v1: The [!UICONTROL API di aggiornamento del profilo bulk] aggiornamenti esistenti [!DNL Target] solo profili. Se utilizzi la versione 1, i profili non vengono creati per mancanti `pcIds` o `mbox3rdPartyIds`.
* [!UICONTROL Attributi del cliente] richiedere l&#39;utilizzo di [!UICONTROL ID EXPERIENCE CLOUD] (ECID) e l’utilizzo di un ID sorgente, ad esempio l’ID del sistema di gestione delle relazioni con i clienti o l’ID fedeltà.
* Il [!UICONTROL API di aggiornamento del profilo bulk] richiede l&#39;ID TNT o `mbox3rdPartyId`.
* Non puoi inviare i seguenti caratteri in `mbox3rdPartyID`: segno più (+) e barra (/).

## Formato

Il file .csv deve fare riferimento a ogni visitatore tramite il rispettivo [!DNL Target] PCID o `mbox3rdPartyId`. Il [!UICONTROL ID EXPERIENCE CLOUD] (ECID) non è supportato. Tutti gli attributi e valori del profilo vengono creati e aggiornati tramite l&#39;API. I dettagli del formato sono disponibili nella documentazione API.

## Casi d’uso di esempio

Il CRM o altro sistema interno memorizza dati preziosi sui visitatori che desideri aggiornare in modo coerente in [!DNL Target], senza esporre i dati del profilo nell’implementazione della pagina.

## Vantaggi del metodo

* Nessun limite al numero di attributi del profilo.
* Gli attributi del profilo inviati tramite il sito possono essere aggiornati tramite l’API e in modo opposto.

## Avvertenze

* La dimensione del file batch deve essere inferiore a 50 MB. Inoltre, il numero totale di righe non deve superare 500000 righe per upload.
* Gli aggiornamenti in genere si verificano in meno di un’ora, ma la visualizzazione potrebbe richiedere fino a 24 ore.
* Non esiste alcun limite al numero o alle righe che è possibile caricare in un periodo di 24 ore nei batch successivi. Tuttavia, il processo di ingestione potrebbe essere limitato durante l&#39;orario di ufficio per garantire che altri processi vengano eseguiti in modo efficiente.
* Chiamate di aggiornamento batch V2 consecutive senza chiamate mbox tra di esse per lo stesso `thirdPartyIds` sovrascrivi le proprietà aggiornate nella prima chiamata di aggiornamento batch.

## Risorse

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)