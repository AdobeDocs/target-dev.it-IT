---
keywords: at.js, funzioni, libreria javascript
description: Visualizza un elenco di funzioni che possono essere utilizzate con le versioni 1.x e 2.x della libreria JavaScript at.js in [!DNL Adobe Target].
title: Quali funzioni posso utilizzare con at.js?
feature: at.js
exl-id: 1efed365-8a74-4c85-bdb1-8daaaf53d642
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 73%

---

# Funzioni di at.js

Elenco di funzioni che possono essere utilizzate con [!DNL Adobe Target] Libreria JavaScript at.js. Per ulteriori informazioni ed esempi, fai clic sui collegamenti nella colonna Funzione.

| Funzione | Dettagli |
| --- | --- | 
| [[!UICONTROL adobe.target.getOffer(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) | Questa funzione genera una richiesta per ottenere un [!DNL Target] offerta. Puoi utilizzarlo con `adobe.target.applyOffer()` per elaborare la risposta o usare la tua gestione di successo. |
| [[!UICONTROL adobe.target.getOffers(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)<P>(at.js 2.x) | Questa funzione ti consente di recuperare più offerte passando più mbox. Inoltre, è possibile recuperare più offerte per tutte le visualizzazioni nelle attività attive.<P>**Nota:** questa funzione è stata introdotta con at.js 2.x e non è disponibile per at.js versione 1.*x*. |
| [[!UICONTROL adobe.target.applyOffer(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) | Questa funzione applica il contenuto di risposta. |
| [[!UICONTROL adobe.target.applyOffers(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)<P>(at.js 2.x) | Questa funzione ti consente di applicare più di un’offerta recuperata da [!UICONTROL adobe.target.getOffers()].<P>**Nota:** questa funzione è stata introdotta con at.js 2.x e non è disponibile per at.js versione 1.*x*. |
| [[!UICONTROL adobe.target.triggerView (viewName, options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)<P>(at.js 2.x) | È possibile chiamare questa funzione a ogni caricamento di una nuova pagina o quando si esegue di nuovo il rendering di un componente di una pagina.<P> Questa funzione deve essere implementata per le applicazioni a pagina singola (SPA) al fine di utilizzare [!UICONTROL Compositore esperienza visivo] (VEC) da creare [!UICONTROL Test A/B] e [!UICONTROL Targeting esperienza] (XT) attività. |
| [[!UICONTROL adobe.target.trackEvent(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) | Questa funzione genera una richiesta per segnalare le azioni dell&#39;utente, ad esempio clic e conversioni. Non recapita le attività nella risposta. |
| [[!UICONTROL mboxCreate(mbox,params)]](/help/dev/implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)<P>(at.js 1.x) | Esegue una richiesta e applica l’offerta all’elemento DIV più vicino con il nome della classe mboxDefault.<P>**Nota:** questa funzione è disponibile per at.js versione 1.*x*. Questa funzione è stata rimossa con il rilascio di at.js 2.x e restituisce il contenuto predefinito se utilizzata con at.js 2.x. |
| [[!UICONTROL mboxDefine(options)] e [!UICONTROL mboxUpdate(options)]](/help/dev/implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)<P>(at.js 1.x) | Definisci e aggiorna una mbox.<P>**Nota:** questa funzione è disponibile per at.js versione 1.*x*. Questa funzione è stata rimossa con il rilascio di at.js 2.x e restituisce il contenuto predefinito se utilizzata con at.js 2.x. |
| [[!UICONTROL targetGlobalSettings(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) | Puoi modificare le impostazioni nella libreria at.js utilizzando `[!UICONTROL targetGlobalSettings()]`, anziché configurarli in [!DNL Target Standard/Premium] Interfaccia utente o utilizzando le API REST.<ul><li>Fornitori di dati: questa impostazione consente ai clienti di raccogliere dati da fornitori di dati terzi, come Demandbase, BlueKai e servizi personalizzati, e di passare i dati a Target come parametri mbox nella richiesta mbox globale.</li></ul> |
| [[!UICONTROL targetPageParams(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) | Questo metodo consente di allegare i parametri alla mbox globale dall’esterno del codice di richiesta. |
| [[!UICONTROL targetPageParamsAll(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparamsall.md) | Questo metodo consente di allegare i parametri a tutte le mbox dall’esterno del codice di richiesta. |
| [[!UICONTROL registerExtension(options)]](/help/dev/implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)<P>(at.js 1.x) | Fornisce un metodo standard per registrare un’estensione specifica.<P>**Nota:** questa funzione è disponibile per at.js versione 1.*x*. Questa funzione è stata rimossa con il rilascio di at.js 2.x e restituisce il contenuto predefinito se utilizzata con at.js 2.x. |
| [[!UICONTROL Eventi personalizzati at.js]](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md) | Eventi personalizzati at.js consentono di sapere quando una richiesta o un’offerta mbox ha esito negativo o positivo. |
| [[!UICONTROL adobe.target.sendNotifications(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)<P>(at.js 2.1.0) | Questa funzione invia una notifica a [!DNL Target] edge quando viene eseguito il rendering di un’esperienza senza utilizzare `[!UICONTROL adobe.target.applyOffer()]` o `[!UICONTROL adobe.target.applyOffers()]`.<P>**Nota**: questa funzione è stata introdotta in at.js 2.1.0 e sarà disponibile per tutte le versioni successive alla versione 2.1.0. |
