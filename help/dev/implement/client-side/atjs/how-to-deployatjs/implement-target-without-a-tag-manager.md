---
keywords: implementare target, implementare, implementare at.js, Tag Manager, decisioning sul dispositivo, su device decisioning
description: Scopri come specificare le impostazioni (dettagli account, metodi di implementazione, ecc.) per implementare la libreria at.js  [!DNL Adobe Target]  senza utilizzare un gestore di tag.
title: Posso implementare  [!DNL Target]  senza un sistema per la gestione dei tag?
feature: Implement Server-side
exl-id: f675ae21-105d-4aa3-9926-59291f1136b5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1693'
ht-degree: 35%

---

# Implementare [!DNL Target] senza un sistema per la gestione dei tag

Informazioni sull&#39;implementazione di [!DNL Adobe Target] senza l&#39;utilizzo di uno o più tag in [!DNL Adobe Experience Platform].

>[!NOTE]
>
>I tag in [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) sono il metodo preferito per implementare [!DNL Target] e la libreria at.js. Le informazioni seguenti non sono applicabili quando si utilizzano i tag in [!DNL Adobe Experience Platform] per implementare [!DNL Target].

Per accedere alla pagina Implementazione, fare clic su **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.

In questa pagina è possibile specificare le impostazioni seguenti:

* Dettagli dell’account
* Metodi di implementazione
* API profilo
* Strumenti di debug
* Privacy

>[!NOTE]
>
>È possibile modificare le impostazioni nella libreria at.js anziché configurarle nell&#39;interfaccia utente di [!DNL Target] Standard/Premium o utilizzando le API REST. Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

## Dettagli dell’account

Puoi visualizzare i seguenti dettagli dell’account. Queste impostazioni non possono essere modificate.

| Impostazione | Descrizione |
| --- | --- |
| [!UICONTROL Client Code] | Il codice client è una sequenza di caratteri specifica del client spesso necessaria quando si utilizzano le API [!DNL Target]. |
| [!UICONTROL IMS Organization ID] | Questo ID collega l’implementazione al tuo account Adobe Experience Cloud. |
| [!UICONTROL On-Device Decisioning] | Per abilitare il decisioning sul dispositivo, fai scorrere l’interruttore su &quot;on&quot;.<p>Le attività di decisioning sul dispositivo consentono di memorizzare nella cache le campagne A/B e Targeting esperienza (XT) sul server ed eseguire attività di decisioning in memoria con latenza pressoché pari a zero. Per ulteriori informazioni, vedere [Introduzione alle decisioni sul dispositivo](../../../server-side/sdk-guides/on-device-decisioning/overview.md). |
| [!UICONTROL Include all existing on-device decisioning qualified activities in the artifact] | (Condizionale) Questa opzione viene visualizzata se abiliti le decisioni sul dispositivo.<p>Attiva l&#39;opzione se desideri includere automaticamente nell&#39;artefatto tutte le attività live di [!DNL Target] idonee per le decisioni su dispositivo.<p>Lasciando questa opzione disattivata, è necessario ricreare e attivare tutte le attività decisionali sul dispositivo affinché vengano incluse nell’artefatto delle regole generato. |

## Metodi di implementazione

Le seguenti impostazioni possono essere configurate nel pannello Metodi di implementazione:

### Impostazioni globali

>[!NOTE]
>
>Queste impostazioni vengono applicate a tutte le [!DNL Target] librerie .js. Dopo aver apportato le modifiche nella sezione Metodi di implementazione, scarica la libreria e aggiornala nella tua implementazione.

| Impostazione | Descrizione |
| --- | --- |
| [!UICONTROL Page load enabled (Auto-create global mbox)] | Seleziona se incorporare la chiamata mbox globale nel file at.js in modo che si attivi automaticamente al caricamento di ogni pagina. |
| [!UICONTROL Global mbox] | Seleziona un nome per la mbox globale. Per impostazione predefinita, il nome è target-global-mbox.<p>Nei nomi delle mbox in at.js è possibile utilizzare caratteri speciali, tra cui il simbolo &amp;. |
| [!UICONTROL Timeout (seconds)] | Se [!DNL Target] non risponde con il contenuto entro il periodo definito, la chiamata al server riceve un timeout e viene visualizzato il contenuto predefinito. Durante la sessione del visitatore vengono ripetuti ulteriori tentativi di chiamata. Il valore predefinito è 5 secondi.<p>La libreria at.js utilizza l&#39;impostazione di timeout in `XMLHttpRequest`. Il timeout viene avviato quando la richiesta viene attivata e si arresta quando [!DNL Target] riceve una risposta dal server. Per ulteriori informazioni, vedere [XMLHttpRequest.timeout](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/timeout) in Mozilla Developer Network.<p>Se il timeout specificato si verifica prima della ricezione della risposta, viene visualizzato il contenuto predefinito e il visitatore potrebbe essere conteggiato come partecipante a un&#39;attività, perché la raccolta dei dati viene eseguita sul server Edge [!DNL Target]. Se la richiesta raggiunge il server Edge [!DNL Target], il visitatore viene conteggiato.<p>Quando configuri l’impostazione di timeout, tieni presente quanto segue:<ul><li>Se il valore è troppo basso, gli utenti potrebbero visualizzare il contenuto predefinito la maggior parte delle volte, nonostante il visitatore venga conteggiato come partecipante all’attività.</li><li>Se il valore è troppo alto, i visitatori potrebbero visualizzare aree vuote nella pagina web o pagine vuote (se utilizzi la funzione per nascondere il corpo) per periodi di tempo prolungati.</li></ul>Per comprendere meglio i tempi di risposta della mbox, guarda la scheda Rete negli strumenti di sviluppo del tuo browser. Puoi anche utilizzare strumenti di terze parti per il monitoraggio delle prestazioni web, ad esempio Catchpoint.<p>**Nota**: l&#39;impostazione [visitorApiTimeout](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#visitorapitimeout) garantisce che [!DNL Target] non attenda troppo a lungo la risposta dell&#39;API visitatore. Questa impostazione e l’impostazione Timeout per at.js qui descritta non entrano in contrasto. |
| [!UICONTROL Profile Lifetime] | Questa impostazione determina per quanto tempo vengono memorizzati i profili visitatore. Per impostazione predefinita, i profili vengono memorizzati per due settimane. Questa impostazione può essere aumentata fino a 90 giorni.<p>Per modificare l&#39;impostazione della durata del profilo, contattare [l&#39;assistenza clienti](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?lang=it#reference_ACA3391A00EF467B87930A450050077C). |

### Metodo di implementazione principale

>[!NOTE]
>
>[!DNL Adobe Target] supporta sia at.js 1.*x* che in at.js 2.*x*. Esegui l’aggiornamento alla versione più recente di una delle versioni principali di at.js per assicurarti di eseguire una versione supportata.

Per scaricare la versione at.js desiderata, fai clic sul pulsante **Scarica** appropriato.

Per modificare l&#39;impostazione di at.js, fai clic su **[!UICONTROL Edit]** accanto alla versione di at.js desiderata.

>[!WARNING]
>
>Prima di modificare queste impostazioni predefinite, consulta [Assistenza clienti](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?lang=it#reference_ACA3391A00EF467B87930A450050077C) in modo da non influire sull&#39;implementazione corrente.

Oltre alle impostazioni spiegate in precedenza, sono disponibili anche le seguenti impostazioni at.js specifiche:

| Impostazione | Descrizione |
|--- |--- |
| Tra domini | Per at.js v1.*x*, specifica se le funzionalità tra domini diversi sono `disabled` (i browser impostano i cookie solo nel tuo dominio (cookie di prime parti), `x only` (i browser impostano i cookie solo nel dominio di Target) o entrambi, selezionando `enabled` (i browser impostano sia i cookie di prima parte che quelli di terze parti). Per at.js v2.10 e versioni successive, specifica se le funzionalità tra domini diversi sono `enabled` (i browser impostano sia cookie di prima parte che di terze parti) o `disabled` (i browser non impostano cookie di terze parti). |
| Intestazione libreria personalizzata | Aggiungi un JavaScript personalizzato da includere nella parte superiore della libreria. |
| Piè di pagina libreria personalizzato | Aggiungi un JavaScript personalizzato da includere nella parte inferiore della libreria. |

### API profilo

Puoi abilitare o disabilitare l’autenticazione per gli aggiornamenti collettivi tramite API e generare un token di autenticazione del profilo.

Per ulteriori informazioni, vedere [Impostazioni API profilo](/help/dev/before-implement/methods-to-get-data-into-target/profile-api-settings.md).

### Strumenti di debug

Generare un token di autorizzazione per utilizzare gli strumenti di debug avanzati di [!DNL Target]. Fare clic su **[!UICONTROL Generate New Authentication Token]**.

![Generare un nuovo token di autenticazione](../../../../before-implement/methods-to-get-data-into-target/assets/debugger-auth-token.png)

### Privacy

Queste impostazioni consentono di utilizzare [!DNL Target] in conformità con le leggi sulla privacy dei dati.

Scegli l’impostazione desiderata dall’elenco a discesa Indirizzo IP visitatore offuscato:

* Offuscamento dell’ultimo ottetto
* Offuscamento dell’intero IP
* None (Nessuno)

Per ulteriori informazioni, consulta [Privacy](/help/dev/before-implement/privacy/privacy.md).

>[!NOTE]
>
>L’opzione Supporto di browser legacy era disponibile nelle versioni 0.9.3 e precedenti di at.js. Questa opzione è stata rimossa in at.js versione 0.9.4. Per un elenco di browser supportati da at.js, consulta [Browser supportati](/help/dev/before-implement/supported-browsers.md).<p>I browser legacy sono browser meno recenti che non supportano completamente la condivisione delle risorse tra diverse origini (Cross Origin Resource Sharing, CORS). Questi browser includono: Internet Explorer nelle versioni precedenti alla versione 11; Safari versione 6 e precedenti. Se Supporto browser legacy è stato disabilitato, [!DNL Target] non ha distribuito contenuto o non ha conteggiato i visitatori nei rapporti di questi browser. Se questa opzione è stata abilitata, si consiglia di eseguire il controllo qualità nei browser più datati per garantire una buona esperienza del cliente.

## Scaricare at.js

Istruzioni per scaricare la libreria utilizzando l&#39;interfaccia [!DNL Target] o l&#39;API di download.

>[!NOTE]
>
>[Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) è il metodo preferito per implementare [!DNL Target] e la libreria at.js. Le informazioni seguenti non sono applicabili quando si utilizzano i tag in [!DNL Adobe Experience Platform] per implementare [!DNL Target].
>
>[!DNL Adobe Target] supporta sia at.js 1.*x* che in at.js 2.*x*. Esegui l’aggiornamento alla versione più recente di una delle versioni principali di at.js per assicurarti di eseguire una versione supportata. Per ulteriori informazioni su ogni versione, consulta [Dettagli sulla versione di at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md).

### Scaricare at.js utilizzando l&#39;interfaccia [!DNL Target]

Per scaricare at.js dall&#39;interfaccia [!DNL Target]:

1. Fare clic su **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.
1. Dalla sezione Metodi di implementazione, fai clic sul pulsante **[!UICONTROL Download]** accanto alla versione at.js desiderata.

### Scarica at.js utilizzando l&#39;API di download [!DNL Target]

Per scaricare at.js utilizzando l’API.

1. Ottieni il tuo codice cliente.

   Il codice client è disponibile nella parte superiore della pagina **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** dell&#39;interfaccia [!DNL Target].

1. Ottieni il tuo numero di amministratore.

   Carica l&#39;URL:

   ```
   https://admin.testandtarget.omniture.com/rest/v1/endpoint/<varname>client code</varname>
   ```

   Sostituisci `client code` con il codice client del passaggio 1.

   Il risultato del caricamento dell&#39;URL deve essere simile al seguente:

   ```
   { 
     "api": "https://admin6.testandtarget.omniture.com/admin/rest/v1" 
   }
   ```

   In questo esempio, “6” è il numero di amministratore.

1. Scarica at.js.

   Carica questo URL con la seguente struttura. Il caricamento di questo URL avvia il download del file at.js personalizzato.

   ```
   https://admin<varname>admin number</varname>.testandtarget.omniture.com/admin/rest/v1/libraries/atjs/download?client=<varname>client code</varname>&version=<version number>
   ```

   * Sostituisci `admin number` con il tuo numero di amministratore.
   * Sostituisci `client code` con il codice client del passaggio 1.
   * Sostituisci `version number` con il numero di versione at.js desiderato (ad esempio, 2.2).

>[!WARNING]
>
>Il team di [!DNL Target] gestisce solo due versioni di at.js: la versione corrente e quella immediatamente precedente. Aggiorna at.js per assicurarti di eseguire una versione supportata. Per ulteriori informazioni su ogni versione, consulta [Dettagli sulla versione di at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md).

## Implementazione di at.js

at.js dovrebbe essere implementato nell’elemento `<head>` di ogni pagina del sito Web.

Un&#39;implementazione tipica di [!DNL Target] che non utilizza un gestore di tag, ad esempio i tag in [Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md), ha un aspetto simile al seguente:

```
<!doctype html> 
<html> 
<head> 
    <meta charset="utf-8"> 
    <title>Title of the Page</title> 
    <!--Preconnect and DNS-Prefetch to improve page load time--> 
    <link rel="preconnect" href="//<client code>.tt.omtrdc.net"> 
    <link rel="dns-prefetch" href="//<client code>.tt.omtrdc.net"> 
    <!--/Preconnect and DNS-Prefetch--> 
    <!--Data Layer to enable rich data collection and targeting--> 
    <script> 
        var digitalData = { 
            "page": { 
                "pageInfo": { 
                    "pageName": "Home" 
                } 
            } 
        }; 
    </script> 
    <!--/Data Layer--> 
    <!-- targetPageParams(), targetPageParamsAll(), Data Providers or targetGlobalSettings() functions to enrich the visitor profile or modify the library settings--> 
    <script> 
        targetPageParams = function() { 
            return { 
                "a": 1, 
                "b": 2, 
                "pageName": digitalData.page.pageInfo.pageName, 
                "profile": { 
                    "age": 26, 
                    "country": { 
                        "city": "San Francisco" 
                    } 
                } 
            }; 
        }; 
    </script> 
    <!--/targetPageParams()--> 
 
    <!--jQuery or other helper libraries should be implemented before at.js if you would like to use their methods in Target--> 
    <script src="jquery-3.3.1.min.js"></script> 
    <!--/jQuery--> 
    <!--Target's JavaScript SDK, at.js--> 
    <script src="at.js"></script> 
    <!--/at.js--> 
</head>
<body> 
    The default content of the page 
</body> 
</html>
```

Considera le seguenti note importanti:

* Deve essere utilizzato il Doctype HTML5, ad esempio `<!doctype html>`. I doctype non supportati o meno recenti potrebbero impedire a [!DNL Target] di effettuare una richiesta.
* Preconnessione e Preacquisizione sono opzioni che potrebbero consentire di caricare più rapidamente le pagine Web. Se utilizzi queste configurazioni, assicurati di sostituire `<client code>` con il tuo codice client, che puoi ottenere dalla pagina **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**.
* Se si dispone di un livello di dati, è ottimale definirlo il più possibile nel `<head>` delle pagine prima di caricare at.js. Questo posizionamento offre la massima capacità di utilizzare queste informazioni in [!DNL Target] per la personalizzazione.
* Le funzioni speciali di [!DNL Target], come `targetPageParams()`, `targetPageParamsAll()`, Data Provider e `targetGlobalSettings()`, devono essere definite dopo il livello dati e prima del caricamento di at.js. In alternativa, queste funzioni possono essere salvate nella sezione Intestazione libreria della pagina Modifica impostazioni at.js e salvate come parte della libreria at.js stessa. Per ulteriori informazioni su queste funzioni, vedi [Funzioni at.js](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md).
* Se si utilizzano librerie di supporto di JavaScript, ad esempio jQuery, includerle prima di [!DNL Target] in modo da poterne utilizzare la sintassi e i metodi durante la creazione di [!DNL Target] esperienze.
* Includere at.js nei `<head>` delle pagine.

## Tracciare le conversioni

Con la mbox di conferma dell’ordine è possibile registrare i dettagli sugli ordini sul sito e generare rapporti in base a ricavi e ordini. Con la mbox di conferma dell’ordine è inoltre possibile determinare algoritmi per i consigli, come “Persone che hanno acquistato il prodotto x hanno acquistato anche il prodotto y”.

>[!NOTE]
>
>Se gli utenti effettuano acquisti sul tuo sito web, Adobe consiglia di implementare una mbox di conferma d&#39;ordine anche se per generare rapporti utilizzi Analytics for [!DNL Target] (A4T).

1. Nella pagina dei dettagli dell’ordine, inserisci lo script mbox secondo il modello indicato di seguito.
1. Sostituisci le parole in lettere maiuscole con valori dinamici o statici provenienti dal catalogo.

   >[!TIP]
   >
   >È inoltre possibile passare le informazioni sull&#39;ordine in qualsiasi mbox (non deve necessariamente essere denominato `orderConfirmPage`). Inoltre, puoi passare le informazioni di ordine in più mbox all&#39;interno della stessa campagna.

   ```
   <script type="text/javascript"> 
   adobe.target.trackEvent({ 
       "mbox": "orderConfirmPage", 
       "params":{  
           "orderId": "ORDER ID FROM YOUR ORDER PAGE",  
           "orderTotal": "ORDER TOTAL FROM YOUR ORDER PAGE",  
           "productPurchasedId": "PRODUCT ID FROM YOUR ORDER PAGE, PRODUCT ID2, PRODUCT ID3"  
       } 
   }); 
   </script> 
   ```

>[!NOTE]
>
>Nella mbox di conferma d&#39;ordine, separa gli ID di più prodotti con la virgola.

La mbox di conferma d&#39;ordine utilizza i seguenti parametri:

| Parametro | Descrizione |
|--- |--- |
| orderId | Valore univoco per identificare un ordine per il conteggio di conversione.<p>L’`orderId` deve essere univoco. Gli ordini duplicati vengono ignorati nei rapporti. |
| orderTotal | Valore monetario dell&#39;acquisto.<p>Non trasmettere il simbolo di valuta. Utilizza un punto decimale (non la virgola) per indicare i valori decimali. |
| productPurchasedId  (facoltativo) | Elenco degli ID dei prodotti acquistati nell&#39;ordine, separati da virgole.<p>Questi ID prodotto vengono visualizzati nel rapporto di audit per supportare ulteriori analisi dei rapporti. |
