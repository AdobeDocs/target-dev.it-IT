---
keywords: adobe.target.triggerView, triggerView, triggerview, trigger view, at.js, funzioni, funzione, viewName, viewname, nome visualizzazione, adobe.target.triggerView1
description: Utilizza la funzione adobe.target.triggerView() per la libreria JavaScript at.js di  [!DNL Adobe Target]  per l'utilizzo in applicazioni a pagina singola (SPA). (at.js 2.x)
title: Come si utilizza la funzione adobe.target.triggerView()?
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: fe4e607173c760f782035a10f52936d96e9db300
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 21%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

È possibile chiamare questa funzione a ogni caricamento di una nuova pagina o quando si esegue di nuovo il rendering di un componente di una pagina. Implementare `adobe.target.triggerView()` per le applicazioni a pagina singola (SPA) per utilizzare [!UICONTROL Visual Experience Composer] (VEC) per creare [!UICONTROL A/B Test] e [!UICONTROL Experience Targeting] (XT) attività. Se `[!UICONTROL adobe.target.triggerView()]` non è implementato sul sito, non è possibile utilizzare il Compositore esperienza visivo per l&#39;SPA. Per ulteriori informazioni, consulta [Implementazione di un’applicazione a pagina singola](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

>[!NOTE]
>
>Questa funzione è stata introdotta con at.js 2.*x*. Questa funzione non è disponibile per at.js versione 1.*x*.

| Parametro | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| viewName | Stringa | Sì | Passa un nome qualsiasi come tipo di stringa che desideri rappresenti la tua visualizzazione. Questo nome della visualizzazione appare nel pannello [!UICONTROL Modifications] del Compositore esperienza visivo per consentire agli addetti al marketing di creare azioni ed eseguire le attività di [!UICONTROL A/B Test] e [!UICONTROL Experience Targeting] XT. |
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

## Esempio: migliore compatibilità per `triggerView()` con [!UICONTROL Adobe Visual Editing Helper extension]

Quando utilizzi l&#39;estensione [Helper per editing video Adobe](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}, considera quanto segue:

A causa dei nuovi criteri V3 Manifest di [!DNL Googl]e per le estensioni [!DNL Chrome], [!UICONTROL Visual Editing Helper extension] deve attendere l&#39;evento `DOMContentLoaded` prima di caricare le librerie [!DNL Target] nel Compositore esperienza visivo. Questo ritardo potrebbe causare l&#39;attivazione della chiamata `triggerView()` da parte delle pagine Web prima che le librerie di authoring siano pronte, con conseguente mancato popolamento della visualizzazione al momento del caricamento.

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


