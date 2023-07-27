---
keywords: Adobe Experience Platform Web SDK, aep web sdk, web sdk, sdk, adobe experience cloud, platform edge network, adobe experience platform edge network, edge network, aep edge network, Adobe Experience Platform Web SDK0
description: Scopri come utilizzare il [!UICONTROL Adobe Experience Platform Web SDK] per interagire con i vari servizi del [!UICONTROL Adobe Experience Cloud] tramite [!UICONTROL AEP Edge Network].
title: Come si implementa con [!UICONTROL Experienci Platform Web SDK]?
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 13%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] (AEP Web SDK) è una libreria JavaScript lato client che consente ai clienti [!UICONTROL Adobe Experience Cloud] per interagire con i vari servizi del [!DNL Adobe Experience Cloud] (incluso [!DNL Target]) tramite [!UICONTROL Adobe Experience Platform Edge Network]. Oltre alla libreria JavaScript, è disponibile [!UICONTROL Adobe Experience Platform] per semplificare le configurazioni del Web SDK.

Per ulteriori informazioni, consulta i seguenti collegamenti nella sezione *[!UICONTROL Adobe Experience Platform Web SDK]* Aiuto:

* Per informazioni complete: [Cos’è [!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=it)
* Per informazioni specifiche su [!DNL Target]: [[!DNL Target] Panoramica](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html?lang=it)

## Esercitazioni

I seguenti tutorial sono utili per la tua implementazione:

### Implementare [!DNL Adobe Experience Cloud] con [!DNL Platform Web SDK]

Scopri come implementare [!DNL Experience Cloud] applicazioni che utilizzano [!DNL Adobe Experience Platform Web SDK] con [questa esercitazione](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=it). Per informazioni specifiche su [!DNL Target], consulta la sezione del tutorial intitolata [Configurazione [!DNL Target] con Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html).

### Migra [!DNL Target] da at.js 2.*x* a [!DNL Platform Web SDK]

Scopri come migrare il tuo [!DNL Target] implementazione da at.js 2.*x* al [!DNL Adobe Experience Platform Web SDK] con [questa esercitazione](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=it).

## Documentazione consigliata

Oltre al [!UICONTROL Platform Web SDK] della documentazione di cui sopra, gli argomenti di questa guida contengono anche informazioni [!UICONTROL Platform Web SDK] in relazione a [!DNL Target] funzionalità.

| Funzione | Descrizione/Collegamento |
| --- | --- |
| [Controllo di qualità attività](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) | Utilizzare gli URL di controllo qualità in [!DNL Target] per eseguire in modo semplice e completo il controllo qualità delle attività, con collegamenti di anteprima che restano invariati, targeting facoltativo del pubblico e rapporti di controllo qualità mantenuti segmentati dai dati delle attività live. Il Controllo di qualità delle attività consente di testare completamente [!DNL Target] attività prima di lanciarle in diretta.<p>Consulta [Compatibilità della modalità QA con la libreria JavaScript di Target](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#compatibility) e [URL di anteprima](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#preview). |
| [[!UICONTROL Analytics for Target] (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) | [!UICONTROL Adobe Analytics for Target] (A4T) è un’integrazione tra soluzioni che consente di creare attività basate su [!DNL Analytics] metriche di conversione e segmenti di pubblico. L’integrazione di A4T consente di utilizzare i rapporti di Analytics per esaminare i risultati.<p>Consulta [Tipi di attività supportati](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html#section_F487896214BF4803AF78C552EF1669AA) e [Passaggi per l’implementazione di Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html#platform). |
| [Tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/target.html) | Tipi di pubblico [!DNL Target] determinare chi visualizzerà i contenuti e le esperienze in un’attività mirata.<p>Consulta [Utilizzare l’elenco Tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#use-list) e [Combinare più tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html). |
| [Creare tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=it) | L’utilizzo dei tipi di pubblico creati in [!DNL Adobe Experience Platform] fornisce dati più completi sui clienti, per una personalizzazione più incisiva.<p>Consulta [Utilizzare i tipi di pubblico da Adobe Experience Platform](https://experienceleague.adobe.com/docs/?lang=ittarget/using/audiences/create-audiences/audiences.html#aep). |
| [Decisioni offerta](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html) | Aggiungere decisioni di offerta create in [!DNL Adobe Journey Optimizer] a [!DNL Target] attività (test A/B manuale o Targeting delle esperienze) per determinare e fornire la migliore offerta successiva per i visitatori su web e dispositivi mobili. |
| [Offerte di reindirizzamento: domande frequenti su A4T](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html) | Le offerte di reindirizzamento fanno sì che i browser dei visitatori reindirizzino a una nuova pagina.<p>Consulta [L&#39; [!UICONTROL Adobe Experience Platform Web SDK] supportare le offerte di reindirizzamento per A4T?](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html#platform) |
| [Token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) | I token di risposta ti consentono di inviare [!DNL Target] dati a Google Analytics e altre integrazioni di terze parti.<p>Consulta [Invio di dati a Google Analytics tramite Platform Web SDK](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html#sending-data-to-google-analytics-via-platform-web-sdk) per visualizzare un esempio di codice per l&#39;esecuzione di questa attività. |
| [Implementazione di un&#39;applicazione a pagina singola](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html) nel *[!UICONTROL Platform Web SDK] panoramica* guida. | [!UICONTROL Adobe Experience Platform Web SDK] offre funzioni avanzate che consentono di eseguire personalizzazioni su tecnologie lato client di nuova generazione, come applicazioni a pagina singola (SPA). |
| [Modifiche alla crittografia di TLS (Transport Layer Security)](../../before-implement/tls-transport-layer-security-encryption.md) | TLS (Transport Layer Security) consente di mantenere standard di sicurezza elevati e di promuovere la sicurezza dei dati dei clienti. |
