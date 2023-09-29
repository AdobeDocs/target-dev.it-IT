---
user-guide-title: Guida per gli sviluppatori di Adobe Target
breadcrumb-title: Guida per gli sviluppatori di Target
user-guide-description: Scopri come adattare e personalizzare l’esperienza dei clienti per massimizzare le entrate dai siti web e mobili, dalle app, dai social media e da altri canali digitali.
source-git-commit: 723bb2f33a011995757009193ee9c48757ae1213
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 53%

---


# Guida per gli sviluppatori di Adobe Target {#developer}

+ [Guida per gli sviluppatori di Adobe Target](overview.md)
+ Introduzione {#implementation}
   + Prima dell’implementazione {#before-implement}
      + [Prima dell’implementazione](before-implement/considerations-before-you-implement-target.md)
      + [Preparare l’implementazione di Target](before-implement/prepare-to-implement-target.md)
   + Privacy e sicurezza {#privacy}
      + [Panoramica sulla privacy](before-implement/privacy/privacy.md)
      + [Normative sulla privacy e la protezione dei dati](before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)
      + [Cookie di Target](before-implement/privacy/cookie-behavior.md)
      + [Eliminare il cookie di Target](before-implement/privacy/cookie-deleting.md)
      + [Criteri per cookie SameSite di Google Chrome](before-implement/privacy/google-chrome-samesite-cookie-policies.md)
      + [Apple Intelligent Tracking Prevention (ITP) 2.x](before-implement/privacy/apple-itp-2x.md)
      + [Direttive Content Security Policy (CSP)](before-implement/privacy/content-security-policy.md)
      + [Elenco consentiti nodi edge di Target](before-implement/privacy/allowlist-edges.md)
   + Metodi per immettere i dati in Target {#methods}
      + [Panoramica dei metodi](before-implement/methods-to-get-data-into-target/methods-to-get-data-into-target.md)
      + [Parametri di pagina](before-implement/methods-to-get-data-into-target/page-parameters.md)
      + [Attributi di profilo nella pagina](before-implement/methods-to-get-data-into-target/in-page-profile-attributes.md)
      + [Attributi di profilo script](before-implement/methods-to-get-data-into-target/script-profile-attributes.md)
      + [Fornitori di dati](before-implement/methods-to-get-data-into-target/data-providers.md)
      + [API di aggiornamento del profilo bulk](before-implement/methods-to-get-data-into-target/bulk-profile-update-api.md)
      + [Aggiornamento di singolo profilo API](before-implement/methods-to-get-data-into-target/single-profile-update-api.md)
      + [Attributi del cliente](before-implement/methods-to-get-data-into-target/customer-attributes.md)
      + [Impostazioni API del profilo](before-implement/methods-to-get-data-into-target/profile-api-settings.md)
   + [Target panoramica sulla sicurezza](before-implement/target-security-overview.md)
   + [Browser supportati](before-implement/supported-browsers.md)
   + [Modifiche alla crittografia di TLS (Transport Layer Security)](before-implement/tls-transport-layer-security-encryption.md)
   + [CNAME e Adobe Target](before-implement/implement-cname-support-in-target.md)
+ Implementazione lato client {#client-side}
   + [Panoramica: Implementare Target per web lato client](implement/client-side/overview.md)
   + [Panoramica sull’implementazione di Adobe Experience Platform Web SDK](implement/client-side/aep-web-sdk.md)
   + Implementazione di at.js {#at-js-implementation}
      + [Panoramica di at.js](implement/client-side/atjs/how-atjs-works/overview.md)
      + Funzionamento di at.js {#at-js}
         + [Panoramica sul funzionamento di at.js](implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
         + [Gestione at.js della visualizzazione momentanea di altri contenuti](implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
         + [Integrazioni at.js](implement/client-side/atjs/how-atjs-works/target-atjs-integrations.md)
      + Come distribuire at.js {#deploy-at-js}
         + [Come distribuire at.js](implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md)
         + [Implementare Target con Adobe Experience Platform](implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)
         + [Implementare Target senza un sistema per la gestione dei tag](implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)
         + [Implementare Target utilizzando Dynamic Tag Manager (DTM)](implement/client-side/atjs/how-to-deployatjs/implement-target-using-dtm.md)
         + [Implementare Target per applicazioni a pagina singola](implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)
      + Decisioning sul dispositivo {#on-device-decisioning}
         + [Panoramica del decisioning sul dispositivo](implement/client-side/atjs/on-device-decisioning/on-device-decisioning.md)
         + [Funzioni supportate](implement/client-side/atjs/on-device-decisioning/supported-features.md)
         + [Artefatto della regola](implement/client-side/atjs/on-device-decisioning/rule-artifact.md)
         + [Risoluzione dei problemi relativi al](implement/client-side/atjs/on-device-decisioning/troubleshooting-on-device-decisioning.md)
      + Funzioni di at.js {#functions-overview}
         + [Panoramica sulle funzioni di at.js](implement/client-side/atjs/atjs-functions/atjs-functions.md)
         + [adobe.target.getOffer()](implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md)
         + [adobe.target.getOffers() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
         + [adobe.target.applyOffer()](implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md)
         + [adobe.target.applyOffers() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)
         + [adobe.target.triggerView() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)
         + [adobe.target.trackEvent()](implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
         + [mboxCreate() - at.js 1.x](implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)
         + [targetGlobalSettings()](implement/client-side/atjs/atjs-functions/targetglobalsettings.md)
         + [mboxDefine() e mboxUpdate() - at.js 1.x](implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)
         + [targetPageParams()](implement/client-side/atjs/atjs-functions/targetpageparams.md)
         + [targetPageParamsAll()](implement/client-side/atjs/atjs-functions/targetpageparamsall.md)
         + [registerExtension() - at.js 1.x](implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)
         + [sendNotifications() - at.js 2.1](implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)
         + [Eventi personalizzati at.js](implement/client-side/atjs/atjs-functions/atjs-custom-events.md)
         + [Eseguire il debug di at.js utilizzando il debugger di Adobe Experience Cloud](implement/client-side/target-debugging-atjs/target-debugging-atjs.md)
         + [Utilizzare istanze basate su cloud con Target](implement/client-side/target-debugging-atjs/targeting-using-cloud-based-instances.md)
      + [FAQ su at.js](implement/client-side/atjs/target-atjs-faq.md)
      + [Dettagli sulle versioni di at.js](implement/client-side/atjs/target-atjs-versions.md)
      + [Aggiornamento da at.js 1.x a at.js 2.x](implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)
      + [Cookie di at.js](implement/client-side/atjs/atjs-cookies.md)
   + [User-agent e suggerimenti client](implement/client-side/atjs/user-agent-and-client-hints.md)
   + Comprendere la mbox globale {#global-mbox}
      + [Panoramica sulla mbox globale](implement/client-side/atjs/global-mbox/global-mbox-overview.md)
      + [Personalizzare una mbox globale](implement/client-side/atjs/global-mbox/customize-global-mbox.md)
      + [Utilizzare una mbox globale da unʼimplementazione legacy](implement/client-side/atjs/global-mbox/mbox-global-target-standard.md)
      + [Trasmettere i parametri a una mbox globale](implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)
      + [Domande frequenti sulla mbox globale](implement/client-side/atjs/global-mbox/global-mbox-faq.md)
+ Implementazione lato server {#server-side}
   + [Lato server: panoramica sull’implementazione di Target](implement/server-side/server-side-overview.md)
   + [Guida introduttiva agli SDK di Target](implement/server-side/sdk-guides/getting-started/getting-started.md)
   + [App di esempio](implement/server-side/sdk-guides/sample-apps/sample-apps.md)
   + [Transizione dalle API legacy di Target ad Adobe I/O](implement/server-side/transition-from-target-classic-apis.md)
   + Principi fondamentali {#core-principles}
      + [Panoramica sui principi fondamentali](implement/server-side/sdk-guides/core-principles/overview.md)
      + [ID utente e bucket](implement/server-side/sdk-guides/core-principles/user-identification-and-bucketing.md)
      + [Targeting del pubblico](implement/server-side/sdk-guides/core-principles/audience-targeting.md)
      + [Tracciamento degli eventi](implement/server-side/sdk-guides/core-principles/event-tracking.md)
      + [Autorizzazioni utente e proprietà](implement/server-side/sdk-guides/core-principles/user-permissions-and-properties.md)
   + Integrazione {#integration}
      + [Panoramica dell’integrazione](implement/server-side/sdk-guides/integration-with-experience-cloud/overview.md)
      + [Servizio ID Experience Cloud (ECID)](implement/server-side/sdk-guides/integration-with-experience-cloud/ecid.md)
      + [Reporting di Analytics for Target (A4T)](implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md)
      + [Segmenti AAM](implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md)
   + Decisioning sul dispositivo {#on-device-decisioning}
      + [Panoramica del decisioning sul dispositivo](implement/server-side/sdk-guides/on-device-decisioning/overview.md)
      + Artefatto della regola {#rule-artifact}
         + [Panoramica dell’artefatto della regola](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)
         + [Scarica tramite SDK di Adobe Target](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-sdk.md)
         + [Download tramite payload JSON](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-json.md)
         + [Esempio di artefatto della regola](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-example.md)
      + [Eseguire test A/B con flag di funzione](implement/server-side/sdk-guides/on-device-decisioning/execute-ab-tests-with-feature-flags.md)
      + [Eseguire test di funzionalità con attributi](implement/server-side/sdk-guides/on-device-decisioning/execute-feature-tests-with-attributes.md)
      + [Gestire i rollout per i test delle funzioni](implement/server-side/sdk-guides/on-device-decisioning/manage-rollouts-for-feature-tests.md)
      + [Distribuire la personalizzazione](implement/server-side/sdk-guides/on-device-decisioning/deliver-personalization.md)
      + [Panoramica delle funzioni supportate](implement/server-side/sdk-guides/on-device-decisioning/supported-features.md)
      + [Risoluzione dei problemi relativi al decisioning sul dispositivo](implement/server-side/sdk-guides/on-device-decisioning/troubleshooting.md)
      + [Best practice](implement/server-side/sdk-guides/best-practices/best-practices.md)
   + Riferimento SDK di Node.js {#node-js}
      + [Panoramica dell’SDK di Node.js](implement/server-side/node-js/overview.md)
      + [Installare l’SDK di Node.js](implement/server-side/node-js/install-sdk.md)
      + [Inizializzare l’SDK di Node.js](implement/server-side/node-js/initialize-sdk.md)
      + [Ottieni offerte (Node.js)](implement/server-side/node-js/get-offers.md)
      + [Ottieni attributi (Node.js)](implement/server-side/node-js/get-attributes.md)
      + [Inviare notifiche (Node.js)](implement/server-side/node-js/send-notifications.md)
      + [Eventi SDK (Node.js)](implement/server-side/node-js/sdk-events.md)
      + [Logger (Node.js)](implement/server-side/node-js/logger.md)
      + [Configurazione proxy (Node.js)](implement/server-side/node-js/proxy-configuration.md)
   + Guida di riferimento dell’SDK Java {#java}
      + [Panoramica dell’SDK Java](implement/server-side/java/overview.md)
      + [Installare l’SDK Java](implement/server-side/java/install-sdk.md)
      + [Inizializzare l’SDK Java](implement/server-side/java/initialize-sdk.md)
      + [Ottieni offerte (Java)](implement/server-side/java/get-offers.md)
      + [Ottieni attributi (Java)](implement/server-side/java/get-attributes.md)
      + [Invio di notifiche (Java)](implement/server-side/java/send-notifications.md)
      + [Eventi SDK (Java)](implement/server-side/java/sdk-events.md)
      + [Logger (Java)](implement/server-side/java/logger.md)
      + [Richieste asincrone (Java)](implement/server-side/java/asynchronous-requests.md)
      + [Configurazione proxy (Java)](implement/server-side/java/proxy-configuration.md)
      + [Configurazione client HTTP personalizzata (Java)](implement/server-side/java/custom-http-client.md)
      + [Metodi di utilità (Java)](implement/server-side/java/utility-methods.md)
   + Riferimento SDK .NET {#net}
      + [Panoramica di .NET SDK](implement/server-side/net/overview.md)
      + [Installare .Net SDK](implement/server-side/net/install-sdk.md)
      + [Inizializzare .NET SDK](implement/server-side/net/initialize-sdk.md)
      + [Ottieni offerte (.NET)](implement/server-side/net/get-offers.md)
      + [Ottieni attributi (.NET)](implement/server-side/net/get-attributes.md)
      + [Invia notifiche (.NET)](implement/server-side/net/send-notifications.md)
      + [Eventi SDK (.NET)](implement/server-side/net/sdk-events.md)
      + [Richieste asincrone (.NET)](implement/server-side/net/asynchronous-requests.md)
   + Riferimenti dell’SDK Python {#python}
      + [Panoramica dell’SDK Python](implement/server-side/python/overview.md)
      + [Installare l’SDK di Python](implement/server-side/python/install-sdk.md)
      + [Inizializzare l’SDK Python](implement/server-side/python/initialize-sdk.md)
      + [Ottieni offerte (Python)](implement/server-side/python/get-offers.md)
      + [Ottieni attributi (Python)](implement/server-side/python/get-attributes.md)
      + [Invia notifiche (Python)](implement/server-side/python/send-notifications.md)
      + [Eventi SDK (Python)](implement/server-side/python/sdk-events.md)
      + [Richieste asincrone (Python)](implement/server-side/python/asynchronous-requests.md)
      + [Logger (Python)](implement/server-side/python/logger.md)
+ [Implementazione ibrida](implement/hybrid/hybrid-overview.md)
+ [Implementazione Recommendations](implement/recommendations/recommendations.md)
+ Implementazione di un’app mobile {#mobile-apps}
   + [Panoramica su Target per le app per dispositivi mobili](implement/mobile/overview.md)
   + [Anteprima mobile di Target](implement/mobile/target-mobile-preview.md)
   + [Utilizzare il servizio posizione](implement/mobile/use-location-service.md)
   + [Domande frequenti su Target per le app per dispositivi mobili](implement/mobile/mobile-faq.md)
   + [Implementare Target con AEP Mobile SDK in un’app nativa con visualizzazioni web](/help/dev/implement/mobile/native-app.md)
+ Implementazione e-mail {#implement-email}
   + [E-mail: panoramica sull’implementazione di Target](implement/email/overview.md)
   + [Creare un AdBox per un’immagine](implement/email/testing-content-with-the-adbox.md)
   + [Test AdBox di un&#39;immagine per e-mail](implement/email/testing-email-image-adbox.md)
   + [Lavorare con i redirector](implement/email/working-with-redirectors.md)
+ Guide API {#api}
   + [Panoramica API di Target](/help/dev/before-administer/target-api-overview.md)
   + [Configurare l’autenticazione per le API di Target](/help/dev/before-administer/configure-authentication.md)
   + Guida all’API di consegna {#delivery-api}
      + [Panoramica dell’API di consegna](/help/dev/implement/delivery-api/overview.md)
      + [SDK per interagire con l’API di consegna](/help/dev/before-implement/delivery-api-overview/sdks.md)
      + [Introduzione](/help/dev/before-implement/delivery-api-overview/getting-started.md)
      + [Autorizzazioni utente (Premium)](/help/dev/before-implement/delivery-api-overview/user-permissions.md)
      + [Identificazione dei visitatori](/help/dev/before-implement/delivery-api-overview/identifying-visitors.md)
      + [Consegna singola o in batch](/help/dev/before-implement/delivery-api-overview/single-or-batch.md)
      + [Preacquisizione](/help/dev/before-implement/delivery-api-overview/prefetch.md)
      + [Notifiche](/help/dev/before-implement/delivery-api-overview/notifications.md)
      + [Integrazione con Experience Cloud](before-implement/delivery-api-overview/integration.md)
      + [Considerazioni e limitazioni note](/help/dev/before-implement/delivery-api-overview/known-limitations.md)
      + [Client Hints](/help/dev/before-implement/delivery-api-overview/client-hints.md)
      + [API di consegna](/help/dev/implement/delivery-api/delivery-api.md)
   + Admin API {#admin-api}
      + [Panoramica dell’API amministratore](before-administer/admin-api-overview/admin-api-overview.md)
      + [API di amministrazione di Adobe Target](/help/dev/administer/admin-api/admin-api-overview-new.md)
   + [API dei profili](/help/dev/administer/profile-api/profile-api-overview.md)
   + [API di reporting](/help/dev/administer/reporting-api/reporting-api.md)
   + API RECOMMENDATIONS {#recommendations-api}
      + [Panoramica API di Recommendations](before-administer/recs-api/overview.md)
      + [Gestire il catalogo con API](before-administer/recs-api/manage-catalog.md)
      + [Gestire i criteri personalizzati](before-administer/recs-api/manage-custom-criteria.md)
      + [Utilizzare l’API di consegna con Recommendations](before-administer/recs-api/fetch-recs-server-side-delivery-api.md)
      + [API RECOMMENDATIONS](/help/dev/administer/recommendations-api/recommendations-api.md)
   + Models API {#models-api}
      + [Panoramica di Models API (Inserisce nell&#39;elenco Bloccati di)](before-administer/models-api.md)
      + [Models API](/help/dev/administer/models-api/models-api-overview.md)
   + [API di Adobe Admin Console](/help/dev/before-implement/delivery-api-overview/adobe-console-api.md)
   + [API server di rete Edge di Adobe Experience Platform](/help/dev/before-implement/delivery-api-overview/aep-edge-network-server-api.md)
+ Modelli di implementazione {#implementation-patterns}
   + [Panoramica sui modelli di implementazione](/help/dev/patterns/pattern-overview.md)
   + Modello di implementazione di Recommendations utilizzando at.js {#atjs}
      + [Panoramica sul modello di implementazione di Recommendations tramite at.js](/help/dev/patterns/recs-atjs/recs-implementation-pattern-atjs.md)
      + [Inizializzare gli SDK](/help/dev/patterns/recs-atjs/initialize-sdk.md)
      + [Configurare la raccolta dati](/help/dev/patterns/recs-atjs/data-collection.md)
      + [Esperienze di rendering](/help/dev/patterns/recs-atjs/render-experiences.md)
      + [Notifica Target](/help/dev/patterns/recs-atjs/notify-target.md)


