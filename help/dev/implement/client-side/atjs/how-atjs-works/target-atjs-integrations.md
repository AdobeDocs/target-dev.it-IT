---
keywords: integrazioni at.js, integrazioni supportate, integrazioni non supportate, integrazioni di terze parti
description: Vedi le integrazioni supportate (e non supportate) da  [!DNL Adobe Target] at.js, incluso [!UICONTROL Analytics for Target] (A4T), [!UICONTROL Experience Cloud ID Service] e altro ancora.
title: Quali integrazioni sono supportate da at.js?
feature: at.js
exl-id: d2c61e77-5fc7-4c35-905b-76b8c4f9df4b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 57%

---

# Integrazioni at.js

Informazioni sulle integrazioni più comuni con [!DNL Adobe Target] e il loro stato di supporto con at.js.

Se hai bisogno di un&#39;integrazione non supportata o menzionata qui, contatta il rappresentante del tuo account o il tuo consulente.

## Integrazioni supportate

| Integrazione | Dettagli |
|--- |--- |
| [!UICONTROL Analytics for Target] (A4T) | Consulta [Adobe Analytics come origine per la generazione di rapporti per Adobe Target (A4T)](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html). |
| [!UICONTROL Profiles & Audiences] (P&amp;A) | Vedi [Tipi di pubblico](https://experienceleague.adobe.com/docs/core-services/interface/audiences/audience-library.html?lang=it) nella *Guida utente dei servizi di base*. |
| [!UICONTROL Experience Cloud ID Service] | Vedi la [documentazione del Servizio Adobe Experience Cloud ID](https://experienceleague.adobe.com/docs/id-service/using/home.html). |
| [!UICONTROL Tags in Adobe Experience Platform] | [!UICONTROL Tags in Adobe Experience Platform] sono la nuova generazione di funzionalità di gestione tag di [!DNL Adobe]. [!UICONTROL Tags] offre ai clienti un modo semplice di implementare e gestire i tag di analisi, marketing e annunci pubblicitari necessari per fornire ai clienti esperienze personalizzate. Vedi [Implementare [!DNL Target] utilizzando Adobe Experience Platform](../how-to-deployatjs/implement-target-using-adobe-launch.md). |
| Cloud Service [!UICONTROL Adobe Experience Manager] (AEM) | [!UICONTROL AEM Cloud Service] abilita la creazione di [!UICONTROL A/B Test] e [!UICONTROL Experience Targeting] attività all&#39;interno del flusso di lavoro AEM. Supporta at.js con [!UICONTROL Adobe Experience Manager] 6.2 con FP-11577 (o versioni successive). Per ulteriori informazioni, vedere [Integrazione con [!DNL Adobe Target]](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it) e selezionare la versione dell&#39;AEM. |
| [!UICONTROL AEM Experience Fragments] | I frammenti di esperienza creati nell&#39;AEM nelle attività [!DNL Target] consentono di combinare la facilità d&#39;uso e la potenza dell&#39;AEM con le potenti capacità di intelligenza automatizzata (AI) e apprendimento automatico (ML) in [!DNL Target] per testare e personalizzare le esperienze su larga scala.  AEM riunisce tutti i contenuti e le risorse in una posizione centrale per alimentare la tua strategia di personalizzazione. AEM consente di creare facilmente contenuti per desktop, tablet e dispositivi mobili in un&#39;unica posizione senza scrivere codice. Non è necessario creare pagine per ogni dispositivo: l’AEM regola automaticamente ogni esperienza utilizzando il contenuto.  Consulta [Frammenti di esperienza AEM](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html). |

## Integrazioni non supportate

| Integrazione | Dettagli |
|--- |--- |
| Integrazione legacy da [!DNL Target] a [!DNL SiteCatalyst] | Questa era l&#39;integrazione che inviava gli ID di campagne e ricette a [!DNL SiteCatalyst] tramite la chiamata alla pagina in modo da poter usare le funzioni di reporting nell&#39;interfaccia utente di [!DNL SiteCatalyst]. A4T sostituisce questa funzionalità. |
| Integrazione legacy da [!DNL Target] a [!DNL SiteCatalyst] | Questa era l’integrazione che effettuava le chiamate mbox `"SiteCatalyst: Event"` e `"SiteCatalyst: Purchase"` che consentivano di generare metriche di successo e profili utente basati su eVar e Prop. A4T e P&amp;A sostituiscono questa funzionalità. |
| Integrazione legacy [!DNL Audience Manager] (AAM) con [!DNL Target] | Questa era l’integrazione che effettuava una chiamata API front-end per recuperare i segmenti AAM e inviarli come parametri mbox per ogni chiamata mbox sulla pagina. |

## Integrazioni di terze parti

| Integrazione | Dettagli |
|--- |--- |
| Altri gestori di tag | at. js dovrebbe funzionare con piattaforme di gestione dei tag non-Adobe, ma presta attenzione quando utilizzi funzionalità di integrazione personalizzate sviluppate da altri fornitori. Le loro integrazioni potrebbero dipendere da funzioni interne di mbox.js che non esistono più in at.js. |
| Provider di dati di terze parti (ad esempio Demandbase, BlueKai, API meteo) | Molti fornitori di dati di terze parti utilizzati per integrare la profilazione utente di Target possono essere integrati utilizzando la funzionalità [Fornitori di dati](../atjs-functions/targetglobalsettings.md#data-providers) di at.js. |
