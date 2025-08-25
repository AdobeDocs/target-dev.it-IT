---
title: Considerazioni sull’API di consegna di Adobe Target e limitazioni note
description: Quali considerazioni e limitazioni note devo tenere in considerazione quando utilizzo [!UICONTROL Adobe Target Delivery API]?
keywords: api di consegna
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 94a4122244065384f487ca9a29dfa1b414168cb8
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 6%

---

# Considerazioni e limitazioni note

Nelle informazioni seguenti sono elencate considerazioni e limitazioni note sull&#39;utilizzo di [!DNL Adobe Target] [!DNL Delivery API].

* Nessuna autenticazione per le API di consegna [!DNL Target].
* Questa API non elabora i cookie o le chiamate di reindirizzamento.
* Sia i nomi di intestazione HTTP/1.1 che HTTP/2 non fanno distinzione tra maiuscole e minuscole; tuttavia, HTTP/2 applica i nomi di intestazione minuscoli. Per ulteriori informazioni, vedere la documentazione del [protocollo HTTP/2 (Hypertext Transfer Protocol Version 2)](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}.

  Se utilizzi un endpoint che indirizza i visitatori attraverso la nostra nuova infrastruttura di load balancer, le loro connessioni vengono automaticamente aggiornate a HTTP/2. Questo processo di aggiornamento converte le intestazioni di richiesta in intestazioni minuscole in modo che non vengano considerate in formato non valido.

  Questo problema può potenzialmente rappresentare un problema per i clienti se le loro librerie sono configurate per cercare intestazioni di richiesta/risposta con distinzione tra maiuscole e minuscole (in particolare non con distinzione tra maiuscole e minuscole).
