---
keywords: adobe.target.triggerView, triggerView, triggerview, trigger view, at.js, funzioni, funzione, viewName, viewname, nome visualizzazione, adobe.target.triggerView1
description: Utilizza la funzione adobe.target.triggerView() per la libreria JavaScript at.js di  [!DNL Adobe Target]  per l'utilizzo in applicazioni a pagina singola. (at.js 2.x)
title: Come si utilizza la funzione adobe.target.triggerView()?
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
TQID: https://experienceleague.adobe.com/pBC1GRKG0mxeaZ1hfaByKv2tu-XScrSJfm7lUw-3yKw
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: a1af9d2c909d9b3d506dd4875d1bd75149dbf636
workflow-type: tm+mt
source-wordcount: 446
ht-degree: 19%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

È possibile chiamare questa funzione a ogni caricamento di una nuova pagina o quando si esegue di nuovo il rendering di un componente di una pagina. Implementare `adobe.target.triggerView()` per le applicazioni a pagina singola per utilizzare il [!UICONTROL Compositore esperienza visivo] per creare [!UICONTROL attività Test A/B] e [!UICONTROL Attività di targeting esperienza] (XT). Se `[!UICONTROL adobe.target.triggerView()]` non è implementato sul sito, non è possibile utilizzare il Compositore esperienza visivo per le applicazioni a pagina singola. Per ulteriori informazioni, consulta [Implementazione di un’applicazione a pagina singola](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

>[!NOTE]
>
>Funzione introdotta con at.js 2.*x*. Funzione non disponibile per at.js versione 1.*x*.

| Parametro | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| viewName | Stringa | Sì | Passa un nome qualsiasi come tipo di stringa che desideri rappresenti la tua visualizzazione. Questo nome della visualizzazione appare nel pannello [!UICONTROL Modifiche] del Compositore esperienza visivo per consentire agli addetti al marketing di creare azioni ed eseguire le attività di [!UICONTROL Test A/B] e [!UICONTROL Targeting esperienza] Targeting esperienza. |
| options | Oggetto | No |  |
| options > page | Booleano | No | **TRUE:** il valore predefinito della pagina è vero. Con page=true, si inviano notifiche al backend [!DNL Target] per incrementare il conteggio delle impression.<P>Una notifica viene sempre inviata per impostazione predefinita quando viene chiamato un `[!UICONTROL triggerView]`, tranne quando options > page è impostato su false.<P>**FALSE:** con page=false, non si inviano le notifiche per incrementare il conteggio delle impression. Usa questo approccio quando vuoi eseguire nuovamente il rendering solo di un componente su una pagina con un’offerta.<P>**Nota**: le offerte di codice personalizzato nel Compositore esperienza visivo non vengono riprodotte quando `[!UICONTROL triggerView()]` viene chiamato con `{page: false}` come opzione. |

## Esempio: True

Chiamata `[!UICONTROL triggerView()]` per inviare una notifica al backend [!DNL Target] per incrementare le impression dell&#39;attività e altre metriche.

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## Esempio: False

Chiamata di `[!UICONTROL triggerView()]` per non inviare notifiche al backend [!DNL Target] per il conteggio delle impression.

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## Esempio: concatenamento promesse con `getoffers()` e `applyOffers()`

Per eseguire `triggerView()` quando la promessa `getOffers()` è risolta, è importante eseguire `triggerView()` sul blocco finale, come illustrato nell&#39;esempio seguente. Questo è necessario affinché il Compositore esperienza visivo possa rilevare `Views` in modalità creazione.

```javascript {line-numbers="true"}
adobe.target.getOffers({
    'request': {
        'prefetch': {
            'views': [{
                'parameters': {}
            }]
        }
    }
}).then(function(response) {
    // Apply Offers
    adobe.target.applyOffers({
        response: response
    });
}).catch(function(error) {
    console.log("AT: getOffers failed - Error", error);
}).finally(() => {
    // Trigger View call, assuming pageView is defined elsewhere
    adobe.target.triggerView(pageView, {
        page: true
    });
    console.log('AT: View triggered on : ' + pageView);
});
```

## Esempio: migliore compatibilità per `triggerView()` con l&#39;estensione [!UICONTROL Adobe Visual Editing Helper]

Quando utilizzi l&#39;estensione [Adobe Visual Editing Helper](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}, tieni presente quanto segue:

A causa dei nuovi criteri V3 Manifest di [!DNL Googl]e per le estensioni [!DNL Chrome], l&#39;estensione [!UICONTROL Helper per editing video] deve attendere l&#39;evento `DOMContentLoaded` prima di caricare le librerie [!DNL Target] nel Compositore esperienza visivo. Questo ritardo potrebbe causare l&#39;attivazione della chiamata `triggerView()` da parte delle pagine Web prima che le librerie di authoring siano pronte, con conseguente mancato popolamento della visualizzazione al momento del caricamento.

Per attenuare questo problema, utilizzare un listener per l&#39;evento pagina `load`.

Di seguito un esempio di implementazione:

```javascript
function triggerViewIfLoaded() {
    adobe.target.triggerView("homeView");
}

if (document.readyState === "complete") {
    // If the page is already loaded
    triggerViewIfLoaded();
} else {
    // If the page is not yet loaded, set up an event listener
    window.addEventListener("load", triggerViewIfLoaded);
}
```





