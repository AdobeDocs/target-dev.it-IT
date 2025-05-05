---
keywords: Adobe Experience Platform Web SDK, aep web sdk, web sdk, sdk, adobe experience cloud, platform edge network, adobe experience platform edge network, edge network, aep edge network, Adobe Experience Platform Web SDK0
description: Scopri come utilizzare [!UICONTROL Adobe Experience Platform Web SDK] per interagire con i vari servizi di [!UICONTROL Adobe Experience Cloud] tramite [!UICONTROL AEP Edge Network].
title: Come posso implementare con [!UICONTROL Experience Platform Web SDK]?
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 8%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] (AEP Web SDK) è una libreria JavaScript lato client che consente ai clienti di [!UICONTROL Adobe Experience Cloud] di interagire con i vari servizi in [!DNL Adobe Experience Cloud] (incluso [!DNL Target]) tramite [!UICONTROL Adobe Experience Platform Edge Network]. Oltre alla libreria JavaScript, è disponibile un&#39;estensione [!UICONTROL Adobe Experience Platform] per le configurazioni dell&#39;SDK Web.

Per ulteriori informazioni, vedere i collegamenti seguenti nella Guida di *[!UICONTROL Adobe Experience Platform Web SDK]*:

* Per informazioni complete: [Cos&#39;è [!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=it)
* Per informazioni specifiche di [!DNL Target]: [[!DNL Target] Panoramica](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html?lang=it)

## Esercitazioni

I seguenti tutorial sono utili per la tua implementazione:

### Implementa [!DNL Adobe Experience Cloud] con [!DNL Platform Web SDK]

Scopri come implementare le applicazioni [!DNL Experience Cloud] utilizzando [!DNL Adobe Experience Platform Web SDK] con [questa esercitazione](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=it). Per informazioni specifiche di [!DNL Target], vedere la sezione dell&#39;esercitazione dal titolo [Configurazione [!DNL Target] con Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html?lang=it).

### Migra [!DNL Target] da at.js 2.*x* a [!DNL Platform Web SDK]

Scopri come migrare l’implementazione di [!DNL Target] da at.js 2.*x* a [!DNL Adobe Experience Platform Web SDK] con [questa esercitazione](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=it).

## Documentazione consigliata

Oltre alla documentazione di [!UICONTROL Platform Web SDK] di cui sopra, gli argomenti di questa guida contengono anche informazioni specifiche di [!UICONTROL Platform Web SDK] in quanto si riferiscono alle funzionalità di [!DNL Target].

| Funzione | Descrizione/collegamento |
| --- | --- |
| [Controllo di qualità attività](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=it) | Gli URL di controllo qualità in [!DNL Target] consentono di verificare in modo semplice e completo la qualità delle attività tramite collegamenti di anteprima che restano invariati, l&#39;eventuale definizione di un pubblico di destinazione e rapporti di controllo qualità mantenuti separati dai dati delle attività live. Controllo di qualità attività consente di testare completamente le attività [!DNL Target] prima di avviarle in tempo reale.<p>Consulta [Compatibilità della modalità QA con la libreria JavaScript di Target](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=it#compatibility) e [URL di anteprima](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=it#preview). |
| [[!UICONTROL Analytics for Target] (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=it) | [!UICONTROL Adobe Analytics for Target] (A4T) è un’integrazione tra soluzioni che consente di creare attività basate su metriche di conversione e segmenti di pubblico di [!DNL Analytics]. L’integrazione di A4T consente di utilizzare i rapporti di Analytics per esaminare i risultati.<p>Consulta [Tipi di attività supportati](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=it#section_F487896214BF4803AF78C552EF1669AA) e [Passaggi di implementazione per un&#39;implementazione di Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html?lang=it#platform). |
| [Tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/target.html?lang=it) | I tipi di pubblico in [!DNL Target] determinano chi visualizzerà il contenuto e le esperienze in un&#39;attività di destinazione.<p>Vedi [Utilizzare l&#39;elenco dei tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=it#use-list) e [Combinare più tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html?lang=it). |
| [Creare tipi di pubblico](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=it) | L&#39;utilizzo dei tipi di pubblico creati in [!DNL Adobe Experience Platform] fornisce dati più completi sui clienti per una personalizzazione di maggiore impatto.<p>Vedi [Utilizza i tipi di pubblico da Adobe Experience Platform](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=it#aep). |
| [Decisioni di offerta](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html?lang=it) | Aggiungi le decisioni sulle offerte create in [!DNL Adobe Journey Optimizer] alle attività [!DNL Target] (test A/B manuale o targeting delle esperienze) per determinare e consegnare la migliore offerta successiva per i visitatori su web e dispositivi mobili. |
| [Offerte di reindirizzamento: domande frequenti su A4T](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=it) | Le offerte di reindirizzamento fanno sì che i browser dei visitatori reindirizzino a una nuova pagina.<p>Vedi [[!UICONTROL Adobe Experience Platform Web SDK] supporta le offerte di reindirizzamento per A4T?](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=it#platform) |
| [Token di risposta](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=it) | I token di risposta ti consentono di inviare dati [!DNL Target] alle Google Analytics e ad altre integrazioni di terze parti.<p>Consulta [Invio di dati alle Google Analytics tramite Platform Web SDK](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=it#sending-data-to-google-analytics-via-platform-web-sdk) per visualizzare un esempio di codice per l&#39;esecuzione di questa attività. |
| [Implementazione di un&#39;applicazione a pagina singola](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html?lang=it) nella *[!UICONTROL Platform Web SDK]panoramica* guida. | [!UICONTROL Adobe Experience Platform Web SDK] offre funzionalità avanzate che consentono all&#39;azienda di eseguire personalizzazioni su tecnologie lato client di nuova generazione, come le applicazioni a pagina singola (SPA). |
| [Modifiche alla crittografia di TLS (Transport Layer Security)](../../before-implement/tls-transport-layer-security-encryption.md) | TLS (Transport Layer Security) consente di mantenere standard di sicurezza elevati e di promuovere la sicurezza dei dati dei clienti. |
