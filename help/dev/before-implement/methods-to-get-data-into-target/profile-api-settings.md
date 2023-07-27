---
keywords: implementazione, api, profilo, impostazioni API profilo, token di autenticazione
description: Scopri come configurare l’autenticazione per gli aggiornamenti batch tramite [!DNL Adobe Target] e generare un token di autenticazione del profilo.
title: Come si utilizzano le impostazioni API del profilo per abilitare o disabilitare gli aggiornamenti in batch?
feature: APIs/SDKs
exl-id: 968f33d0-296b-4248-8c9a-8e6f3077bdfa
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 42%

---

# Impostazioni API del profilo

Abilita o disabilita l&#39;autenticazione per gli aggiornamenti batch tramite [!DNL Adobe Target] e generare un token di autenticazione del profilo.

[!DNL Adobe Target] crea e conserva un profilo per ogni singolo utente. Questo profilo è memorizzato in [!DNL Target] edge cluster e viene aggiornato in tempo reale dopo ogni visita. Puoi anche aggiornare un profilo singolarmente o in blocco tramite API.

Per una maggiore sicurezza, puoi scegliere che l’intestazione della richiesta di aggiornamento collettivo dell’API debba contenere un token di accesso valido.

**[!DNL Target]Per richiedere l’autenticazione e generare un token di accesso utilizzando l’interfaccia utente di :**

1. Fai clic su **[!UICONTROL Amministrazione]** > **[!UICONTROL Implementazione]**.
1. Sotto **[!UICONTROL API profilo]** far scorrere il **[!UICONTROL Richiedi autenticazione]** passa alla posizione abilitato o disabilitato.

   ![immagine alt](assets/profile_api_settings.png)

1. (Condizionale) Se hai attivato i requisiti di autenticazione, fai clic su **[!UICONTROL Genera nuovo token di autenticazione profilo]**.

   ![immagine alt](assets/profile_api_settings_2.png)

   La scadenza del token è indicata nella casella Scade tra.

   Per generare un token di autenticazione, è necessario disporre di una delle seguenti autorizzazioni utente:

   * Ruolo amministratore o almeno con diritti approvatore

     Per ulteriori informazioni per i clienti di Target Standard, consulta [Specificare ruoli e autorizzazioni](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html#roles-permissions) in *Utenti*. Per ulteriori informazioni per i clienti [!DNL Target Premium], consulta [Configurare le autorizzazioni aziendali](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

   * Ruolo amministratore a livello di area di lavoro/profilo di prodotto

     Le aree di lavoro sono disponibili solo per i clienti [!DNL Target Premium]. Per ulteriori informazioni, consulta [Configurare le autorizzazioni aziendali](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

   * Diritti di amministrazione (autorizzazione Sysadmin) a livello di prodotto [!DNL Adobe Target]

Puoi anche generare un token di autenticazione profilo tramite API. Per ulteriori informazioni, consulta &quot;Profili&quot; in [Guida dell’API per l’amministrazione e il profilo di Adobe Target](../../administer/admin-api/admin-api-overview-new.md).

1. Copia il token e includilo nell’intestazione della richiesta nel formato: &quot;Authorization&quot; : &quot;Bearer&quot;.

1. Clic **[!UICONTROL Genera nuovo token di autenticazione profilo]** per rigenerare il token in base alle esigenze.

>[!WARNING]
>
>Se si reimposta il token, le chiamate API che utilizzano il token attuale non riusciranno. Sarà necessario aggiornare tutti gli script o le applicazioni che utilizzano tale token.
