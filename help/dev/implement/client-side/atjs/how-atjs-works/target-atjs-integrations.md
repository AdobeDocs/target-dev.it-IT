---
keywords: integrazioni at.js, integrazioni supportate, integrazioni non supportate, integrazioni di terze parti
description: Vedi le integrazioni supportate (e non supportate) da  [!DNL Adobe Target] at.js, incluso [!UICONTROL Analytics for Target] (A4T), [!UICONTROL Experience Cloud ID Service] e altro ancora.
title: Quali integrazioni sono supportate da at.js?
feature: at.js
exl-id: d2c61e77-5fc7-4c35-905b-76b8c4f9df4b
TQID: https://experienceleague.adobe.com/RdcxcIGufo2O5aKPqIAJVINkCzZ1Brcv8EXiX1n4buc
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: df62f171-ac37-440f-8f0f-f41a72ebdd34
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: e6ff21d3-dec6-4298-8590-7c749fffaf78
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 486
ht-degree: 56%

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
| Cloud Service [!UICONTROL Adobe Experience Manager] (AEM) | [!UICONTROL AEM Cloud Service] abilita la creazione di [!UICONTROL A/B Test] e [!UICONTROL Experience Targeting] attività all&#39;interno del flusso di lavoro di AEM. Supporta at.js con [!UICONTROL Adobe Experience Manager] 6.2 con FP-11577 (o versioni successive). Per ulteriori informazioni, consulta [Integrazione con [!DNL Adobe Target]](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=it) e seleziona la tua versione di AEM. |
| [!UICONTROL AEM Experience Fragments] | I frammenti di esperienza creati in AEM nelle attività [!DNL Target] consentono di combinare la facilità d&#39;uso e la potenza di AEM con le potenti capacità di intelligenza automatizzata (AI) e apprendimento automatico (ML) di [!DNL Target] per testare e personalizzare le esperienze su larga scala.  AEM riunisce tutti i contenuti e le risorse in una posizione centrale per alimentare la tua strategia di personalizzazione. AEM consente di creare facilmente contenuti per desktop, tablet e dispositivi mobili in un&#39;unica posizione senza scrivere codice. Non è necessario creare pagine per ogni dispositivo: AEM regola automaticamente ogni esperienza utilizzando i contenuti.  Consulta [Frammenti di esperienza AEM](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html). |

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
