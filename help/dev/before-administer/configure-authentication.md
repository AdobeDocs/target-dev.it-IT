---
title: 'Come configurare l''autenticazione per le API  [!DNL Adobe Target] '
description: Come si generano i token di autenticazione necessari per interagire correttamente con le API  [!DNL Adobe Target] ?
feature: APIs/SDKs, Administration & Configuration
exl-id: fc67363c-6527-40aa-aff1-350b5af884ab
TQID: https://experienceleague.adobe.com/sgdBKse1b-0kPKjzDx4fDoFsNpnIzXAT8TpDUkQ7fGw
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: addda914fcf7ba1616ae9a9d49118e737b3ad923
workflow-type: tm+mt
source-wordcount: 1927
ht-degree: 1%

---

# Configura autenticazione per le API [!DNL Adobe Target]

Le API dell&#39;amministratore [!DNL Adobe Target], incluse le API [!DNL Recommendations Admin], sono protette dall&#39;autenticazione per garantire che solo gli utenti autorizzati possano utilizzarle per accedere a [!DNL Adobe Target]. Utilizza [Adobe Developer Console](https://developer.adobe.com/console/home) per gestire questa autenticazione per tutti i [!DNL Adobe Experience Cloud solutions], incluso [!DNL Adobe Target].

>[!IMPORTANT]
>
>Le credenziali dell’account di servizio (JWT) descritte in questo articolo diventeranno obsolete e verranno sostituite dalle nuove credenziali server-to-server OAuth.
>
>Le credenziali dell’account di servizio (JWT) continueranno a funzionare fino al 1° gennaio 2025. È necessario migrare l’applicazione o l’integrazione per utilizzare le nuove credenziali server-to-server OAuth prima del 1° gennaio 2025.
>
>Per ulteriori informazioni e istruzioni dettagliate per la migrazione dell&#39;integrazione, vedere [Migrazione delle credenziali dall&#39;account di servizio (JWT) alle credenziali server-to-server OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/){target=_blank} nella documentazione di *Developer Console*.
>
>Per informazioni sulla configurazione delle nuove credenziali OAuth, consulta [Implementazione delle credenziali da server a server OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/){target=_blank} nella documentazione di *Developer Console*.

Di seguito sono riportati i passaggi preliminari necessari per generare i token di autenticazione JWT legacy necessari per interagire correttamente con le API [!DNL Adobe Target]:

1. Creare un progetto (precedentemente denominato integrazione) in [!DNL Adobe Developer Console].
1. Esporta dettagli progetto in Postman.
1. Genera un token di accesso bearer.
1. Verifica il token di accesso bearer.

## Prerequisiti

| Risorsa | Dettagli |
| --- | --- |
| Postman | Per completare correttamente questi passaggi, ottieni l&#39;[app Postman](https://www.postman.com/downloads/) per il tuo sistema operativo. Postman basic è gratuito con la creazione dell&#39;account. Anche se non è necessario per utilizzare le API di [!DNL Adobe Target] in generale, Postman semplifica i flussi di lavoro API e [!DNL Adobe Target] fornisce diverse raccolte Postman per aiutarle a eseguire le API e a capire come funzionano. Il resto di questa guida presuppone una conoscenza operativa di Postman. Per assistenza, consulta la [documentazione di Postman](https://learning.getpostman.com/). |
| Riferimenti | Per il resto di questa guida si presume che le risorse seguenti siano familiari:<ul><li>[Github Adobe I/O](https://github.com/adobeio)</li><li>[Documentazione API per l&#39;amministrazione di Target e il profilo](../administer/admin-api/admin-api-overview-new.md)</li><li>[Documentazione API per la funzione Consigli](https://developer.adobe.com/target/administer/recommendations-api/)</li></ul> |

## Creazione di un progetto Adobe I/O

In questa sezione, accederai a [!DNL Adobe Developer Console] e creerai un progetto per [!DNL Adobe Target]. Per ulteriori informazioni, consulta la [documentazione sui progetti](https://developer.adobe.com/developer-console/docs/guides/projects/).

<!--(1. Generate your private key and public certificate, per the [documentation on authentication](https://developer.adobe.com/developer-console/docs/guides/authentication/). // [//]: # (as described in **Step 1** of [How to set up Adobe IO: Authentication - Step by Step](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html). After completing Step 1, return to this guide and resume with Step 2, below. // The outcome of this step should be the creation of a `private.key` file and a `certificate_pub.crt` file. Return to this guide once you have generated these two files.)-->

1. In [Adobe Admin Console](https://adminconsole.adobe.com/), assicurati che all&#39;account utente [!DNL Adobe] sia stato concesso l&#39;accesso di livello [Amministratore prodotto](https://helpx.adobe.com/enterprise/using/admin-roles.html) e [Sviluppatore](https://helpx.adobe.com/enterprise/using/manage-developers.html) a [!DNL Target].

1. In [Adobe Developer Console](https://developer.adobe.com/console/home), seleziona la [!UICONTROL organizzazione Experience Cloud] per la quale desideri creare questa integrazione. È probabile che tu abbia accesso a una sola [!UICONTROL organizzazione Experience Cloud].

   ![configure-io-target-createproject2.png](assets/configure-io-target-createproject2.png)

1. Fare clic su **[!UICONTROL Crea nuovo progetto]**.

   ![configure-io-target-createproject3.png](assets/configure-io-target-createproject3.png)

1. Fai clic su **[!UICONTROL Aggiungi API]** per aggiungere un&#39;API REST al progetto per accedere ai servizi e ai prodotti [!DNL Adobe].

   ![Aggiungi API](assets/configure-io-target-createproject4.png)

1. Selezionare **[!DNL Adobe Target]** come servizio [!DNL Adobe] da integrare con. Fai clic sul pulsante **[!UICONTROL Avanti]** visualizzato.

   ![configure-io-target-createproject5](assets/configure-io-target-createproject5.png)

1. Selezionare un&#39;opzione per associare le chiavi pubbliche e private all&#39;integrazione dell&#39;account del servizio che si sta creando per [!DNL Target]. Per questo esempio, seleziona **[!UICONTROL Opzione 1: genera una coppia di chiavi]** e fai clic su **[!UICONTROL Genera coppia di chiavi]**.

   ![configure-io-target-createproject6](assets/configure-io-target-createproject6.png)

1. Come indicato, prendere nota del file di configurazione scaricato automaticamente (`config`), che contiene la chiave privata. Fai clic su **[!UICONTROL Avanti]**.

   ![configure-io-target-createproject7](assets/configure-io-target-createproject7.png)

1. Nel file system verificare il percorso di `config`, ovvero il file di configurazione compresso creato nel passaggio precedente. Anche in questo caso, il file `config` contiene la chiave privata, che sarà necessaria in seguito. La posizione esatta all’interno del file system potrebbe essere diversa da quella mostrata qui.

   ![configure-io-target-createproject8](assets/configure-io-target-createproject8.png)

1. In Adobe Developer Console, seleziona i [profili di prodotto](https://helpx.adobe.com/it/enterprise/using/manage-products-and-profiles.html) corrispondenti alle proprietà in cui utilizzi Adobe Recommendations. Se non si utilizzano le proprietà, selezionare l&#39;opzione Default Workspace. Fai clic su **[!UICONTROL Salva API configurata]**.

   ![configure-io-target-createproject9](assets/configure-io-target-createproject9.png)

1. Fare clic su **[!UICONTROL Crea integrazione]**. Dovresti ricevere un messaggio temporaneo che indica che l’API è stata configurata correttamente.
1. Come ultimo passaggio, rinomina il progetto con un nome più significativo dell&#39;originale `Project 1`. A tale scopo, passare al progetto utilizzando il percorso di navigazione come mostrato, fare clic su **[!UICONTROL Modifica progetto]** per accedere al modale **[!UICONTROL Modifica progetto]** e rinominare il progetto.

   ![configure-io-target-createproject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
>
>In questo esempio, il progetto viene denominato &quot;[!DNL Target] Integration.&quot; Se prevedi di utilizzare il progetto per più di [!DNL Adobe Target], potrebbe essere utile denominarlo di conseguenza. Ad esempio, puoi scegliere di denominarla &quot;API di Adobe&quot; o &quot;API di Experience Cloud&quot;, in quanto può essere utilizzata con altre soluzioni di Adobe Experience Cloud.

## Esporta dettagli progetto

Ora che disponi di un progetto Adobe utilizzabile per accedere a [!DNL Target], devi assicurarti di inviare i dettagli del progetto insieme alle richieste API di Adobe. Questi dettagli sono necessari per interagire con diverse API di Adobe, tra cui diverse API di [!DNL Target]. Ad esempio, i dettagli dell&#39;integrazione includono le informazioni di autorizzazione e autenticazione richieste dalle API amministratore [!DNL Target]. Pertanto, per utilizzare le API con Postman, è necessario inserire tali dettagli in Postman.

Esistono diversi modi per specificare i dettagli del progetto in Postman, ma in questa sezione sfrutta alcune funzioni e raccolte predefinite. Innanzitutto (in questa sezione), esporterai i dettagli dell’integrazione in un ambiente Postman. Nella sezione successiva verrà generato un token di accesso Bearer per consentire l’accesso alle risorse Adobe necessarie.

>[!NOTE]
>
>Per le istruzioni video applicabili a qualsiasi soluzione Experience Cloud, incluso [!DNL Target], vedi [Utilizzare Postman con le API di Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html). Le sezioni seguenti sono rilevanti per le API [!DNL Target]: 1. Creare ed esportare l’API di Experience Platform in Postman 2. Generare un token di accesso con Postman. Questi passaggi sono descritti anche di seguito.

1. Sempre in [Adobe Developer Console](https://developer.adobe.com/console/home), passa a visualizzare le credenziali dell&#39;account di servizio **[!UICONTROL del nuovo progetto]**. Utilizza la navigazione a sinistra o la sezione **[!UICONTROL Credenziali]** come mostrato.

   ![JWT1](assets/configure-io-target-jwt1.png)

   In **[!UICONTROL Dettagli credenziali]**, tieni presente che puoi visualizzare le tue **[!UICONTROL Chiavi pubbliche]**, **[!UICONTROL ID client]** e altre informazioni relative al tuo account di servizio.

   ![JWT1a](assets/configure-io-target-jwt1a.png)

1. Fare clic per passare alle informazioni sull&#39;API **[!DNL Adobe Target]**. Utilizza la navigazione a sinistra o la sezione **Prodotti e servizi connessi** come mostrato.

   ![JWT2](assets/configure-io-target-jwt2.png)

1. Fai clic su **[!UICONTROL Scarica per Postman]** > **[!UICONTROL Account di servizio (JWT)]** per creare un file JSON che acquisisce le informazioni di autenticazione per un ambiente Postman.

   ![JWT3](assets/configure-io-target-jwt3.png)

   Prendi nota del file JSON nel file system.

   ![JWT3a](assets/configure-io-target-jwt3a.png)

1. In Postman, fai clic sull&#39;icona a forma di ingranaggio per gestire gli ambienti, quindi fai clic su **[!UICONTROL Importa]** per importare il file JSON (ambiente).

   ![JWT4](assets/configure-io-target-jwt4.png)

1. Scegli il tuo file e fai clic su **[!UICONTROL Apri]**.

   ![JWT5](assets/configure-io-target-jwt5.png)

1. Nella finestra modale **Gestisci ambienti** di Postman, fai clic sul nome dell&#39;ambiente appena importato per esaminarlo. (Il nome dell’ambiente potrebbe essere diverso da quello mostrato qui. Modifica il nome come desiderato. Non deve necessariamente corrispondere al nome del progetto [!DNL Adobe].)

   ![JWT6](assets/configure-io-target-jwt6.png)

1. I valori delle note `CLIENT_SECRET` e `API_KEY` (insieme ad altre variabili) sono precompilati, presi dall&#39;integrazione definita in Adobe Developer Console. La variabile Postman `CLIENT_SECRET` deve corrispondere alle credenziali Adobe `CLIENT SECRET` visualizzate in Developer Console e `API_KEY` in Postman deve corrispondere a `CLIENT ID` in Developer Console. Le note `PRIVATE_KEY`, `JWT_TOKEN` e `ACCESS_TOKEN` sono invece vuote. Iniziamo fornendo il valore `PRIVATE_KEY`.

   ![JWT7](assets/configure-io-target-jwt7.png)

1. Dal file system, aprire il file `config` e aprire il file di chiave `private`.

   ![JWT8](assets/configure-io-target-jwt8.png)

1. Selezionare e copiare l&#39;intero contenuto del file di chiave `private`.

   ![JWT9](assets/configure-io-target-jwt9.png)

1. In Postman, incolla il valore della chiave privata nei campi **[!UICONTROL VALORE INIZIALE]** e **[!UICONTROL VALORE CORRENTE]**.

   ![JWT10](assets/configure-io-target-jwt10.png)

1. Fai clic su **[!UICONTROL Aggiorna]** e chiudi la finestra modale Ambienti.

## Genera il token di accesso bearer

In questa sezione viene generato il token di accesso bearer, necessario per autenticare l&#39;interazione con le API [!DNL Adobe Target]. Per generare il token di accesso bearer, devi inviare i dettagli di integrazione (stabiliti nelle sezioni precedenti) al [servizio Adobe Identity Management (IMS)](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md). Esistono alcuni modi diversi per farlo, ma in questa guida utilizziamo una raccolta Postman contenente una chiamata IMS predefinita che rende il processo diretto e semplice. Dopo aver importato la raccolta, puoi riutilizzarla quando necessario, per generare nuovi token non solo per [!DNL Adobe Target], ma anche per altre API di Adobe.

1. Passa alle [chiamate di esempio dell&#39;API di Adobe Identity Management Service](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims).

   ![token1](assets/configure-io-target-generatetoken1.png)

1. Fare clic sulla **[!UICONTROL raccolta Postman di generazione token di accesso Adobe I/O]**.

   ![token2](assets/configure-io-target-generatetoken2.png)

1. Ottieni il JSON non elaborato per questa raccolta facendo clic su **[!UICONTROL Raw]**, quindi copiando il JSON risultante negli Appunti. In alternativa, puoi salvare il file JSON non elaborato come file .json.

   ![token3](assets/configure-io-target-generatetoken3.png)

1. In Postman, importa la raccolta incollando e inviando il JSON non elaborato dagli Appunti. In alternativa, puoi caricare il file .json salvato. Fate clic su **[!UICONTROL Continue]** (Continua).

   ![token4](assets/configure-io-target-generatetoken4.png)

1. Seleziona la richiesta **[!UICONTROL IMS: JWT Generate + Auth via User Token]** nella raccolta Postman di generazione dei token di accesso di Adobe I/O, assicurati che l&#39;ambiente sia selezionato e fai clic su **[!UICONTROL Send]** per generare il token.

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >Questo token di accesso al bearer sarà valido per 24 ore. Invia nuovamente la richiesta ogni volta che devi generare un nuovo token.

1. Apri nuovamente la finestra modale Manage Environments (Gestisci ambienti) e seleziona l’ambiente.

   ![token6](assets/configure-io-target-jwt11.png)

1. Nota: i valori `ACCESS_TOKEN` e `JWT_TOKEN` sono ora compilati.

   ![token7](assets/configure-io-target-generatetoken7.png)

Domanda: devo utilizzare la raccolta Postman di generazione del token di accesso di Adobe I/O per generare il token web JSON (JWT) e il token di accesso bearer?

Risposta: No. La raccolta Postman per la generazione di token di accesso di Adobe I/O è disponibile per semplificare la generazione del token di accesso JWT e Bearer in Postman. In alternativa, puoi utilizzare le funzionalità di Adobe Developer Console per generare manualmente il token di accesso bearer.

## Verifica il token di accesso bearer

In questo esercizio utilizzerai il nuovo token di accesso bearer inviando una richiesta API per recuperare un elenco di attività dal tuo account [!DNL Target]. Una risposta corretta indica che il progetto [!DNL Adobe] e l&#39;autenticazione funzionano come previsto per utilizzare l&#39;API.

1. Importa la [[!DNL Adobe Target] raccolta Postman API amministratore](https://developers.adobetarget.com/api/#admin-postman-collection). Segui tutti i prompt finché la raccolta non viene importata in Postman.

   ![testtoken1](assets/configure-io-target-testtoken0.png)

1. Espandere la raccolta e prendere nota della richiesta **[!UICONTROL Attività elenco]**.

   ![testtoken1](assets/configure-io-target-testtoken1.png)

1. Variabili come `{{access_token}}` inizialmente non risolte. È possibile risolvere il problema in diversi modi, ad esempio definendo una nuova variabile di raccolta denominata `{{access_token}}`. In questa guida verrà invece modificata la richiesta API per sfruttare l&#39;ambiente Postman utilizzato in precedenza. Questo consentirà all’ambiente di continuare a fungere da consolidamento unico e coerente di tutte le variabili comuni tra le API di Adobe.

   ![testtoken2](assets/configure-io-target-testtoken2.png)

1. Digitare per sostituire `{{access_token}}` con `{{ACCESS_TOKEN}}`.

   ![testtoken3](assets/configure-io-target-testtoken3.png)

1. Digitare per sostituire `{{api_key}}` con `{{API_KEY}}`.

   ![testtoken4](assets/configure-io-target-testtoken4.png)

1. Digitare per sostituire `{{tenant}}` con `{{TENANT_ID}}`. Nota `{{TENANT_ID}}` non ancora riconosciuta.

   ![testtoken4](assets/configure-io-target-testtoken4a.png)

1. Apri la finestra modale Manage Environments (Gestisci ambienti) e seleziona l’ambiente.

   ![JWT11](assets/configure-io-target-jwt11.png)

1. Digitare per aggiungere una nuova variabile di ambiente `{{TENANT_ID}}`. Copia e incolla il valore ID tenant nei campi **[!UICONTROL VALORE INIZIALE]** e **[!UICONTROL VALORE CORRENTE]** per la nuova variabile di ambiente `TENANT_ID`.

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >L&#39;ID tenant è diverso da [!DNL Target] `clientcode`. L&#39;ID tenant esiste nell&#39;URL al momento dell&#39;accesso a [!DNL Target]. Per ottenere l&#39;ID tenant, accedi ad Adobe Experience Cloud, apri [!DNL Target] e fai clic sulla scheda Target. Utilizza il valore ID tenant indicato nel sottodominio URL. Ad esempio, se l&#39;URL al momento dell&#39;accesso a [!DNL Adobe Target] è `<https://mycompany.experiencecloud.adobe.com/...>`, l&#39;ID tenant sarà &quot;mycompany&quot;.

1. Invia la richiesta, dopo aver verificato di aver selezionato l’ambiente corretto. Dovresti ricevere una risposta contenente l’elenco delle attività.

   ![testtoken6](assets/configure-io-target-testtoken6.png)

Dopo aver verificato l&#39;autenticazione Adobe, è possibile utilizzarla per interagire con le API [!DNL Adobe Target] (nonché con altre API Adobe). Ad esempio, puoi [utilizzare le API Recommendations](recs-api/overview.md) per creare o gestire i consigli, oppure puoi utilizzarle con [API di consegna Target](/help/dev/implement/delivery-api/overview.md).
