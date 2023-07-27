---
title: Considerazioni sull’API di consegna di Adobe Target e limitazioni note
description: Quali considerazioni e limitazioni note devo tenere in considerazione quando utilizzo il [!UICONTROL API di consegna di Adobe Target]?
keywords: api di consegna
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 7%

---

# Considerazioni e limitazioni note

* Nessuna autenticazione per [!DNL Target] API di consegna.
* Questa API non elabora i cookie o le chiamate di reindirizzamento.
* Supporto per [!UICONTROL Automated Personalization] (AP) [!UICONTROL Recommendations] attività: questa API dispone di due modalità per recuperare il contenuto: modalità di esecuzione e modalità di preacquisizione. La modalità di preacquisizione può essere utilizzata solo per [!UICONTROL Test AB] e [!UICONTROL Targeting esperienza] (XT) attività. Non utilizzare la modalità di preacquisizione per [!UICONTROL Automated Personalization] AP) [!UICONTROL Allocazione automatica], [!UICONTROL Targeting automatico], o [!UICONTROL Recommendations] tipi di attività.
* Sia i nomi di intestazione HTTP/1.1 che HTTP/2 non fanno distinzione tra maiuscole e minuscole; tuttavia, HTTP/2 applica i nomi di intestazione minuscoli. Per ulteriori informazioni, vedere [Documentazione di Hypertext Transfer Protocol versione 2 (HTTP/2)](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}.

  Se utilizzi un endpoint che indirizza i visitatori attraverso la nostra nuova infrastruttura di load balancer, le loro connessioni vengono automaticamente aggiornate a HTTP/2. Questo processo di aggiornamento converte le intestazioni di richiesta in intestazioni minuscole in modo che non vengano considerate in formato non valido.

  Questo problema può potenzialmente rappresentare un problema per i clienti se le loro librerie sono configurate per cercare intestazioni di richiesta/risposta con distinzione tra maiuscole e minuscole (in particolare non con distinzione tra maiuscole e minuscole).
