---
keywords: adobe.target.triggerView, triggerView, triggerview, trigger view, at.js, funzioni, funzione, viewName, viewname, nome visualizzazione, adobe.target.triggerView1
description: Utilizza la funzione adobe.target.triggerView() per [!DNL Adobe Target] Libreria JavaScript at.js da utilizzare nelle applicazioni a pagina singola (SPA). (at.js 2.x)
title: Come si utilizza la funzione adobe.target.triggerView()?
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 37%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

È possibile chiamare questa funzione a ogni caricamento di una nuova pagina o quando si esegue di nuovo il rendering di un componente di una pagina. `adobe.target.triggerView()` devono essere implementate per le applicazioni a pagina singola (SPA) per utilizzare [!UICONTROL Compositore esperienza visivo] (VEC) da creare [!UICONTROL Test A/B] e [!UICONTROL Targeting esperienza] (XT) attività. Se `[!UICONTROL adobe.target.triggerView()]` non è implementato sul sito, il Compositore esperienza visivo non può essere utilizzato per l’SPA. Per ulteriori informazioni, consulta [Implementazione di un’applicazione a pagina singola](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).

>[!NOTE]
>
>Questa funzione è stata introdotta con at.js 2.*x*. Questa funzione non è disponibile per at.js versione 1.*x*.

| Parametro | Tipo | Obbligatorio | Descrizione |
| --- | --- | --- | --- |
| viewName | Stringa | Sì | Passa un nome qualsiasi come tipo di stringa che desideri rappresenti la tua visualizzazione. Questo nome della visualizzazione viene visualizzato nel [!UICONTROL Modifiche] del Compositore esperienza visivo per consentire agli addetti al marketing di creare azioni ed eseguire [!UICONTROL Test A/B] e [!UICONTROL Targeting esperienza] Attività XT. |
| options | Oggetto | No |  |
| options > page | Booleano | No | **TRUE:** il valore predefinito della pagina è vero. Con page=true, si inviano notifiche al backend [!DNL Target] per incrementare il conteggio delle impression.<P>Una notifica viene sempre inviata per impostazione predefinita quando `[!UICONTROL triggerView]` viene chiamato, tranne quando options > page è impostato su false.<P>**FALSE:** con page=false, non si inviano le notifiche per incrementare il conteggio delle impression. Usa questo approccio quando vuoi eseguire nuovamente il rendering solo di un componente su una pagina con un’offerta.<P>**Nota**: le offerte di codice personalizzato nel Compositore esperienza visivo non vengono riprodotte quando `[!UICONTROL triggerView()]` viene chiamato con `{page: false}` come opzione. |

## Esempio: True

Chiamata `[!UICONTROL triggerView()]`[!DNL Target] per inviare una notifica al backend per incrementare le impression dell’attività e altre metriche.

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## Esempio: False

Chiamata `[!UICONTROL triggerView()]`[!DNL Target] per non inviare notifiche al backend per il conteggio delle impression.

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## Esempio: concatenamento promesse con `getoffers()` e `applyOffers()`

Da eseguire `triggerView()` quando `getOffers()` promessa risolta, è importante eseguire `triggerView()` sul blocco finale, come illustrato nell’esempio seguente. Questo è necessario affinché il Compositore esperienza visivo possa rilevare `Views` in modalità authoring.

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
