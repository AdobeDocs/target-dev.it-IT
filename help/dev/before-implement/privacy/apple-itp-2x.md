---
keywords: apple, ITP, prevenzione del tracciamento intelligente, experience cloud id, ecid, itp
description: Scopri [!DNL Adobe Target]  e l'impatto dell'iniziativa ITP (Intelligent Tracking Prevention) di Apple che mira a proteggere la privacy degli utenti Safari.
title: In che modo  [!DNL Target] gestisce il supporto ITP di Apple?
feature: Privacy & Security
exl-id: 6deee03b-df86-4d0d-999c-b11855ddfda5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 30%

---

# Apple Intelligent Tracking Prevention (ITP) 2.x

Intelligent Tracking Prevention (ITP) è l’iniziativa di Apple per proteggere la privacy degli utenti Safari. La prima versione di ITP, rilasciata nel 2017, interessava l’uso dei cookie di terze parti. In pratica, Apple bloccava tutti i cookie di terze parti, il che causava non poche complicazioni per aziende di tecnologie per pubblicità e marketing, perché i cookie di terze parti venivano generalmente utilizzati per tenere traccia dei visitatori e raccoglierne i dati. Ora Apple sta implementando limiti e restrizioni sul modo in cui i cookie di prima parte vengono utilizzati in Safari.

Queste versioni di ITP includono le seguenti restrizioni:

| Versione | Dettagli |
| --- | --- |
| [ITP 2.1](https://webkit.org/blog/8613/intelligent-tracking-prevention-2-1/) | Ai cookie lato client inseriti nel browser utilizzando l’API `document.cookie` viene applicata una scadenza di sette giorni.<br />Data di rilascio: 21 febbraio 2019. |
| [ITP 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/) | La scadenza di sette giorni è stata drasticamente ridotta a un giorno.<br />Data di rilascio: 24 aprile 2019. |
| [ITP 2.3](https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/) | Sono state eliminate diverse soluzioni alternative, ad esempio l&#39;utilizzo di localStorage o di JavaScript `Document.referrer property`.<br />Data di rilascio: 23 settembre 2019.<br />Funzionalità di difesa in maschera CNAME per ITP rilasciata in Safari 14, macOS Big Sur, Catalina, Mojave, iOS 14 e iPadOS 14. Tutti i cookie creati da una risposta HTTP mascherata da CNAME di terze parti scadranno tra sette giorni.<br />Annunciato il 12 novembre 2020. |

## Qual è l&#39;impatto per il cliente [!DNL Target]?

Target fornisce le librerie JavaScript da distribuire sulle pagine in modo che [!DNL Target] possa offrire ai tuoi visitatori una personalizzazione in tempo reale. Sono disponibili tre [!DNL Target] librerie JavaScript at.js 1.*x*, at.js 2.*x*, le [!DNL Adobe Experience Cloud Web SDK] che inseriscono cookie [!DNL Target] lato client nei browser dei visitatori tramite l&#39;API `document.cookie`. Di conseguenza, i cookie di [!DNL Target] sono interessati da Apple ITP 2.1, 2.2 e 2.3 e scadranno rispettivamente dopo sette giorni (con ITP 2.1) e dopo un giorno (con ITP 2.2 e ITP 2.3).

Apple ITP 2.x ha un impatto su [!DNL Target] nelle seguenti aree:

| Impatto | Dettagli |
| --- | --- |
| Aumento potenziale dei conteggi di visitatori univoci | Poiché la finestra di scadenza è impostata su sette giorni (con ITP 2.1) e un giorno (con ITP 2.2 e ITP 2.3), potrebbe verificarsi un aumento di visitatori univoci provenienti dai browser Safari. Se i visitatori visitano di nuovo il tuo dominio dopo sette giorni (ITP 2.1) o un giorno (ITP 2.2 e ITP 2.3), [!DNL Target] è costretto a inserire un nuovo cookie [!DNL Target] nel tuo dominio al posto del cookie scaduto. Il nuovo cookie [!DNL Target] dà luogo a un nuovo visitatore univoco, nonostante si tratti sempre dello stesso visitatore. |
| Periodi di lookback ridotti per le attività [!DNL Target] | I profili dei visitatori per le attività [!DNL Target] potrebbero avere un periodo di lookback ridotto per scopi decisionali. I cookie [!DNL Target] vengono utilizzati per identificare un visitatore e archiviare gli attributi del profilo utente ai fini della personalizzazione. Dato che i cookie [!DNL Target] possono scadere su Safari dopo sette giorni (ITP 2.1) o un giorno (ITP 2.2 e 2.3), i dati del profilo utente associati al cookie [!DNL Target] eliminato non possono essere più utilizzati a fini decisionali. |
| Script di profilo basati su 3rdPartyID | Poiché la finestra di scadenza è impostata su sette giorni (con ITP 2.1) e un giorno (con ITP 2.2 e ITP 2.3), [gli script di profilo](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=it) basati sul cookie 3rdPartyID cesseranno di funzionare alla scadenza. |
| URL di controllo qualità/anteprima nei dispositivi iOS | Poiché l&#39;intervallo di scadenza è impostato su sette giorni (con ITP 2.1) e un giorno (con ITP 2.2 e ITP 2.3), [gli URL di controllo qualità/anteprima](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=it) cesseranno di funzionare alla scadenza perché gli URL sono basati sul cookie 3rdPartyID. |

## La mia attuale implementazione [!DNL Target] è interessata?

Se utilizzi la libreria Experience Cloud ID (ECID) oltre alla libreria JavaScript [!DNL Target], la tua implementazione sarà interessata come descritto in questo : [Safari ITP 2.1 Impact on Adobe Experience Cloud and Experience Platform Customers](https://medium.com/adobetech/safari-itp-2-1-impact-on-adobe-experience-cloud-customers-9439cecb55ac).
