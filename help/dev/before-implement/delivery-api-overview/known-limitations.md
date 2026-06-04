---
title: Considerazioni sull’API di consegna di Adobe Target e limitazioni note
description: Quali considerazioni e limitazioni note devo tenere in considerazione quando utilizzo l'[!UICONTROL API di consegna di Adobe Target]?
keywords: API Delivery
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/LCgGZONQpYfw6JxNCNc2Iu13Mft8Zfx-3Uxvm2EeUVk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 158
ht-degree: 6%

---

# Considerazioni e limitazioni note

Nelle informazioni seguenti sono elencate considerazioni e limitazioni note sull&#39;utilizzo di [!DNL Adobe Target] [!DNL Delivery API].

* Nessuna autenticazione per le API di consegna [!DNL Target].
* Questa API non elabora i cookie o le chiamate di reindirizzamento.
* Sia i nomi di intestazione HTTP/1.1 che HTTP/2 non fanno distinzione tra maiuscole e minuscole; tuttavia, HTTP/2 applica i nomi di intestazione minuscoli. Per ulteriori informazioni, vedere la documentazione del [protocollo HTTP/2 (Hypertext Transfer Protocol Version 2)](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}.

  Se utilizzi un endpoint che indirizza i visitatori attraverso la nostra nuova infrastruttura di load balancer, le loro connessioni vengono automaticamente aggiornate a HTTP/2. Questo processo di aggiornamento converte le intestazioni di richiesta in intestazioni minuscole in modo che non vengano considerate in formato non valido.

  Questo problema può potenzialmente rappresentare un problema per i clienti se le loro librerie sono configurate per cercare intestazioni di richiesta/risposta con distinzione tra maiuscole e minuscole (in particolare non con distinzione tra maiuscole e minuscole).
