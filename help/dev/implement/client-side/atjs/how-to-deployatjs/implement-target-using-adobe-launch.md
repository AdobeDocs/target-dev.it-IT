---
keywords: implementazione, implementazione, implementazione, adobe launch, launch, race, redirect, platform launch di esperienze, platform launch, tag, piattaforma adobe, implementazione2
description: Scopri come implementare la libreria at.js  [!DNL Adobe Target]  utilizzando  [!DNL Adobe Experience Platform], il metodo preferito per implementare Target.
title: Come si implementa  [!DNL Target]  utilizzando  [!DNL Adobe Experience Platform]?
feature: Implement Server-side
exl-id: 0a325871-194a-479c-a3bf-294e3dde3e9a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 59%

---

# Implementazione di [!DNL Target] tilizzando [!DNL Adobe Experience Platform]

I tag in [!DNL Adobe Experience Platform] rappresentano la nuova generazione di funzionalità [!DNL Adobe] per la gestione dei tag. I tag offrono ai clienti un modo semplice di implementare e gestire i tag di analisi, marketing e annunci pubblicitari necessari per fornire ai clienti esperienze personalizzate.

>[!NOTE]
>
>Adobe Experience Platform Launch è stato ridefinito come suite di tecnologie di raccolta dati in [!DNL Adobe Experience Platform]. Di conseguenza, sono state introdotte diverse modifiche terminologiche nella documentazione del prodotto. Per un riferimento consolidato delle modifiche terminologiche, consulta il seguente [documento](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?).

Nella tabella seguente sono elencate diverse risorse utili:

| Risorsa | Dettagli |
|--- |--- |
| [Aggiungi Adobe Target](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/implement-solutions/target.html?lang=it#implement-solutions) | Questo tutorial fornisce istruzioni dettagliate per implementare [!DNL Target] in un sito web con tag in [!DNL Adobe Experience Platform]. Gli argomenti includono l’aggiunta della libreria JavaScript at.js, l’attivazione della mbox globale, l’aggiunta di parametri e l’integrazione con altre soluzioni. Questo articolo fa parte di un tutorial più ampio che mostra come implementare Adobe Experience Platform e altre soluzioni Adobe Experience Cloud. |
| [Guida rapida](https://experienceleague.adobe.com/docs/experience-platform/tags/get-started/quick-start.html?lang=it) | Informazioni sull’implementazione e la gestione dei tag di analisi, marketing e annunci pubblicitari necessari per fornire ai clienti esperienze personalizzate. |
| [Panoramica dell’estensione Adobe  [!DNL Target] ](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/target/overview.html?lang=it) | Informazioni sull’implementazione di [!DNL Target] mediante [!DNL Adobe Experience Platform]. |

## Vantaggi dell’implementazione di at.js con l’estensione [!DNL Target]

I seguenti vantaggi si applicano solo se si utilizzano i tag in [!DNL Adobe Experience Platform] per implementare at.js. Per questo motivo, Adobe consiglia vivamente di utilizzare i tag in [!DNL Adobe Experience Platform] anziché l&#39;implementazione manuale di at.js.

* **Risolve una situazione di tipo “race condition” tra [!DNL Adobe Analytics] e [!DNL Target]:** poiché la chiamata di [!DNL Analytics] potrebbe essere attivata prima della chiamata di [!DNL Target], la chiamata di [!DNL Target] non viene unita alla chiamata di [!DNL Analytics] e la sequenza potrebbe generare dei dati errati. L’estensione [!DNL Target] fa sì che la chiamata beacon di [!DNL Analytics] attenda che venga completata la chiamata di [!DNL Target], a prescindere dal suo esito positivo o negativo. L’utilizzo dei tag in [!DNL Adobe Experience Platform] evita le possibili incongruenze nei dati derivanti da un’implementazione manuale.

  >[!NOTE]
  >
  >Utilizzare l&#39;azione Invia beacon nell&#39;estensione [!DNL Adobe Analytics] in modo che la chiamata [!DNL Analytics] attenda la chiamata [!DNL Target]. Se chiami direttamente `s.t()` o `s.tl()` con un codice personalizzato, le chiamate di [!DNL Analytics] non attendono il completamento delle chiamate di [!DNL Target].

* **Impedisce la gestione errata delle offerte di reindirizzamento:** Se sulla pagina sono presenti [!DNL Target] e [!DNL Analytics] ed è presente un&#39;offerta di reindirizzamento eseguita da Target, si potrebbero riscontrare dei problemi se il tracker [!DNL Analytics] genera una richiesta quando non dovrebbe (perché l&#39;utente viene reindirizzato a un URL diverso). Se implementi [!DNL Target] e [!DNL Analytics] tramite i tag in [!DNL Adobe Experience Platform], questo problema non si verifica. Quando si utilizzano i tag in [!DNL Adobe Experience Platform], [!DNL Target] indica ad [!DNL Analytics] di interrompere la richiesta beacon di [!DNL Analytics].
