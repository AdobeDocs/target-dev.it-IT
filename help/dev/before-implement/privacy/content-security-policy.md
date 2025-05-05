---
keywords: informativa sulla sicurezza dei contenuti, csp, at.js, whitelist, inserisco nell'elenco Consentiti di, visualizzazione momentanea di altri contenuti, pre-hiding, pre-hiding, informativa sulla sicurezza dei contenuti, iFrame, iframe
description: Scopri le direttive Content Security Policy (CSP) da aggiungere quando utilizzi  [!DNL Adobe Target].
title: In che modo  [!DNL Target]  gestisce le direttive Content Security Policy (CSP)?
feature: Privacy & Security
exl-id: ec6942e5-36d8-4f88-b3d6-47f9eaca03a8
source-git-commit: c43c79b29768694eac534e22047b5ee6a3d0ccd5
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 28%

---

# Direttive Content Security Policy (CSP)

Se utilizzi [Content Security Policy](https://en.wikipedia.org/wiki/Content_Security_Policy) (CSP) per l&#39;implementazione di [!DNL Adobe Target], aggiungi le seguenti direttive CSP quando utilizzi [at.js 2.1 o versione successiva](../../implement/client-side/atjs/target-atjs-versions.md):

* `connect-src` con `*.tt.omtrdc.net` inserito nell’elenco Consentiti. Necessario per consentire la richiesta di rete all’edge [!DNL Target].
* `style-src unsafe-inline`. Necessario per il controllo di pre-hiding e sfarfallio.
* `script-src unsafe-inline`. Necessario per consentire l’esecuzione di JavaScript che potrebbe far parte di un’offerta HTML.

## Domande frequenti

Domande frequenti sui criteri di sicurezza:

### I criteri di condivisione delle risorse tra diverse origini (Cross Origin Resource Sharing, CORS) e i criteri Flash tra più domini presentano problemi di sicurezza?

L’implementazione consigliata del criterio CORS consiste nel consentire l’accesso solo alle origini affidabili che lo richiedono, inserendo i domini affidabili in un elenco Consentiti. Lo stesso vale per il criterio Flash tra domini diversi. Alcuni clienti di [!DNL Target] hanno dubbi in merito all&#39;utilizzo di caratteri jolly nei domini in Target. Il problema è che, se un utente ha effettuato l’accesso a un’applicazione e visita un dominio consentito dal criterio, eventuali contenuti dannosi in esecuzione su tale dominio possono potenzialmente recuperare contenuti sensibili dall’applicazione ed eseguire azioni all’interno del contesto di sicurezza dell’utente connesso. Questa situazione viene comunemente definita CSRF (Cross-Site Request Forgery).

In un&#39;implementazione di [!DNL Target], tuttavia, questi criteri non devono rappresentare un problema di sicurezza.

“adobe.tt.omtrdc.net” è un dominio di proprietà di Adobe. [!DNL Adobe Target] è uno strumento di test e personalizzazione. In quanto tale, è previsto che [!DNL Target] possa ricevere ed elaborare richieste provenienti da ovunque senza richiedere alcuna autenticazione. Tali richieste contengono coppie di chiave/valore utilizzate per test A/B, consigli o personalizzazione dei contenuti.

Adobe non memorizza dati personali (PII, Personally Identifiable Information) né altri dati sensibili sui server perimetrali [!DNL Adobe Target] a cui fa riferimento &quot;adobe.tt.omtrdc.net&quot;.

È previsto che [!DNL Target] possa essere accessibile da qualsiasi dominio tramite chiamate JavaScript. L’unico modo per consentire questo accesso consiste nell’applicare &quot;Access-Control-Allow-Origin&quot; con un carattere jolly.

### Come posso consentire o impedire che il mio sito venga incorporato come iFrame in domini esterni?

Per consentire al [Compositore esperienza visivo](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=it){target=_blank} (VEC) di incorporare il sito Web in un iFrame, è necessario modificare la CSP (se impostata) nell&#39;impostazione del server Web. [!DNL Adobe] domini devono essere inseriti nella whitelist e configurati.

Per motivi di sicurezza, potresti voler impedire che il tuo sito venga incorporato come iFrame in domini esterni.

Nelle sezioni seguenti viene illustrato come consentire o impedire al Compositore esperienza visivo di incorporare il sito in un iFrame.

#### Consentire al Compositore esperienza visivo di incorporare il sito in un iFrame

La soluzione più semplice per consentire al Compositore esperienza visivo di incorporare il sito web in un iFrame è quella di consentire `*.adobe.com`, che è il carattere jolly più ampio.

Ad esempio:

`Content-Security-Policy: frame-ancestors 'self' *.adobe.com`

Come nell’illustrazione seguente (fai clic per ingrandire):


![CSP con carattere jolly più ampio](/help/dev/before-implement/privacy/assets/csp-adobe.png){width="600" zoomable="yes"}

È possibile consentire solo il servizio [!DNL Adobe] effettivo. Questo scenario può essere ottenuto utilizzando `*.experiencecloud.adobe.com + https://experiencecloud.adobe.com`.

Ad esempio:

`Content-Security-Policy: frame-ancestors 'self' https://*.experiencecloud.adobe.com https://experiencecloud.adobe.com https://experience.adobe.com`

Come nell’illustrazione seguente (fai clic per ingrandire):

![CSP con ambito Experience Cloud](/help/dev/before-implement/privacy/assets/csp-experiencecloud.png){width="600" zoomable="yes"}

L&#39;accesso più restrittivo all&#39;account di un&#39;azienda può essere ottenuto utilizzando `https://<Client Code>.experiencecloud.adobe.com https://experience.adobe.com`, dove `<Client Code>` rappresenta il codice client specifico.

Ad esempio:

`Content-Security-Policy: frame-ancestors 'self'  https://ags118.experiencecloud.adobe.com https://experience.adobe.com`

Come nell’illustrazione seguente (fai clic per ingrandire):

![CSP con ambito clientcode](/help/dev/before-implement/privacy/assets/csp-clientcode.png){width="600" zoomable="yes"}

>[!NOTE]
>
>Se hai implementato [Launch/Tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md), anche questo deve essere sbloccato.
>
>Ad esempio:
>
> `Content-Security-Policy: frame-ancestors 'self' *.adobe.com *.assets.adobedtm.com;`

#### Impedire al Compositore esperienza visivo di incorporare il sito in un iFrame

Per evitare che il Compositore esperienza visivo incorpori il sito in un iFrame, puoi limitare l’opzione solo a &quot;self&quot; (self).

Ad esempio:

`Content-Security-Policy: frame-ancestors 'self'`

Come mostrato nella figura seguente (fare clic per ingrandire):

![Errore CSP](/help/dev/before-implement/privacy/assets/csp-error.png){width="600" zoomable="yes"}

Viene visualizzato il seguente messaggio di errore:

`Refused to frame 'https://kuehl.local/' because an ancestor violates the following Content Security Policy directive: "frame-ancestors 'self'".`

