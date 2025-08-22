---
title: Panoramica dell’API di consegna di Adobe Target
description: Panoramica dell’API di consegna di Adobe Target
keywords: api di consegna
exl-id: e760bddc-b1ae-4b7b-bff2-aba81c6b6d34
feature: APIs/SDKs
source-git-commit: ccc27e66207e58dcd33865e5d28a51644e8e1931
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 0%

---

# Panoramica dell’API di consegna

[!DNL Adobe Target Delivery API] è basato su REST. Questa documentazione descrive le risorse che compongono [!DNL Adobe Target] [!DNL Delivery API]. I metodi HTTP vengono utilizzati per eseguire operazioni su tali risorse.

Utilizzando [!UICONTROL Adobe Target's Delivery API], è possibile:

* Distribuisci esperienze su web, incluse applicazioni a pagina singola, canali mobili e dispositivi IoT non basati su browser, ad esempio TV connessa, chiosco o schermi digitali in-store.
* Distribuisci esperienze da qualsiasi piattaforma o applicazione lato server in grado di effettuare chiamate HTTP/s.
* Fornisci esperienze coerenti e personalizzate a un utente, indipendentemente dal canale o dai dispositivi coinvolti dall’utente nella tua attività.
* Memorizza nella cache le esperienze di un utente all’interno di una sessione nel server in modo da evitare più chiamate API e, di conseguenza, ottenere prestazioni migliori.
* Integrazione perfetta con [!DNL Adobe Experience Cloud] prodotti come [!DNL Adobe Analytics], [!DNL Adobe Audience Manager] e [!DNL Experience Cloud ID Service] dal lato server.

>[!IMPORTANT]
>
>Presta attenzione quando aggiorni [!DNL Recommendations] [!UICONTROL Catalog] tramite [!DNL Delivery API]. [!DNL Delivery API] è pubblico, quindi evita di utilizzarlo per popolare gli elementi cliccabili nel catalogo dei consigli. In questo modo si possono introdurre contenuti invalidati e inquinare il catalogo.
>
>Best practice:
>
>Utilizza [!DNL Delivery API] solo per aggiornare gli attributi del catalogo che:
>* Cambia frequentemente (ad esempio, prezzo, livello di azioni).
>* Segui un formato predefinito che può essere facilmente convalidato sul tuo sito web.
>* Non utilizzarlo per aggiungere o modificare elementi cliccabili o altri contenuti non verificati.
>
>Se necessario, puoi richiedere all’assistenza clienti di disabilitare gli aggiornamenti del catalogo tramite l’API di consegna.

Per ulteriori informazioni, vedere la documentazione di [[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank}.

>[!NOTE]
>
>Puoi comunque accedere alla [documentazione legacy /v1/mbox e /v2/batchmbox API](https://developers.adobetarget.com/api/legacy-api/index.html). Tuttavia, nell’API di consegna verranno sviluppate delle funzioni (come documentato qui) che non verranno supportate nelle API legacy.
