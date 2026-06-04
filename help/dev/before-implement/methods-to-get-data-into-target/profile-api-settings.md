---
keywords: implementazione, api, profilo, impostazioni API profilo, token di autenticazione
description: Scopri come configurare l’autenticazione per gli aggiornamenti batch tramite API  [!DNL Adobe Target]  e generare un token di autenticazione del profilo.
title: Come si utilizzano le impostazioni API del profilo per abilitare o disabilitare gli aggiornamenti in batch?
feature: APIs/SDKs
exl-id: 968f33d0-296b-4248-8c9a-8e6f3077bdfa
TQID: https://experienceleague.adobe.com/-KYSphaCrm0ICK7g92v9x-uK--nwirs4-DWBR3G5rTM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 363
ht-degree: 31%

---

# Impostazioni API del profilo

Attivare o disattivare l&#39;autenticazione per gli aggiornamenti batch tramite API [!DNL Adobe Target] e generare un token di autenticazione profilo.

[!DNL Adobe Target] crea e gestisce un profilo per ogni singolo utente. Questo profilo è archiviato nel cluster Edge [!DNL Target] e viene aggiornato in tempo reale dopo ogni visita. Puoi anche aggiornare un profilo singolarmente o in blocco tramite API.

Per una maggiore sicurezza, puoi scegliere che l’intestazione della richiesta di aggiornamento collettivo dell’API debba contenere un token di accesso valido.

**Per richiedere l&#39;autenticazione e generare un token di accesso tramite l&#39;interfaccia utente [!DNL Target]:**

1. Fare clic su **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]**.
1. Nella diapositiva **[!UICONTROL API profilo]**, **[!UICONTROL Richiedi autenticazione]** passa alla posizione abilitata o disabilitata.

   ![Alt immagine](assets/profile_api_settings.png)

1. (Condizionale) Se hai abilitato il requisito di autenticazione, fai clic su **[!UICONTROL Genera nuovo token di autenticazione profilo]**.

   ![Alt immagine](assets/profile_api_settings_2.png)

   La scadenza del token è indicata nella casella Scade tra.

   Per generare un token di autenticazione, è necessario disporre di una delle seguenti autorizzazioni utente:

   * Ruolo amministratore o almeno con diritti approvatore

     Per ulteriori informazioni per i clienti Target Standard, vedere [Specificare ruoli e autorizzazioni](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html#roles-permissions) in *Utenti*. Per ulteriori informazioni per i clienti [!DNL Target Premium], consulta [Configurare le autorizzazioni aziendali](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

   * Ruolo amministratore a livello di area di lavoro/profilo di prodotto

     Le aree di lavoro sono disponibili solo per i clienti [!DNL Target Premium]. Per ulteriori informazioni, consulta [Configurare le autorizzazioni aziendali](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

   * Diritti di amministrazione (autorizzazione Sysadmin) a livello di prodotto [!DNL Adobe Target]

Puoi anche generare un token di autenticazione profilo tramite API. Per ulteriori informazioni, vedere &quot;Profiles&quot; nella [Guida dell&#39;amministratore Adobe Target e dell&#39;API dei profili](../../administer/admin-api/admin-api-overview-new.md).

1. Copia il token e includilo nell’intestazione della richiesta nel formato: &quot;Authorization&quot; : &quot;Bearer&quot;.

1. Fai clic su **[!UICONTROL Genera nuovo token di autenticazione profilo]** per rigenerare il token in base alle esigenze.

>[!WARNING]
>
>Se si reimposta il token, le chiamate API che utilizzano il token attuale non riusciranno. Sarà necessario aggiornare tutti gli script o le applicazioni che utilizzano tale token.
