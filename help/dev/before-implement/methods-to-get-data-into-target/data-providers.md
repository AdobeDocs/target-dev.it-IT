---
keywords: implementare, implementare, configurare, configurare, provider di dati
description: Ottieni dati in [!DNL Target] utilizzando i provider di dati.
title: Come posso inserire dati in [!DNL Target] utilizzando i provider di dati?
feature: Implementation
exl-id: 9971bd96-f736-4965-afe2-b4901c12d006
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 55%

---

# Fornitori di dati

I provider di dati sono una funzionalità che consente di passare facilmente i dati da terze parti a [!DNL Adobe Target].

>[!NOTE]
>
>I provider di dati richiedono at.js 1.3 o versione successiva.

## Formato

L&#39;impostazione `window.targetGlobalSettings.dataProviders` è un array dei fornitori di dati.

Per ulteriori informazioni sulla struttura di ciascun fornitore di dati, vedi [Fornitori di dati](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

## Casi d’uso di esempio

Un esempio di terza parte potrebbe essere un servizio meteo, un DMP o persino il tuo servizio web. Puoi utilizzare questi dati per generare tipi di pubblico e contenuti mirati e per arricchire il profilo del visitatore.

## Vantaggi del metodo

Questa impostazione consente ai clienti di raccogliere dati da provider di dati di terze parti, come Demandbase, BlueKai e servizi personalizzati, e di passare i dati a [!DNL Target] come parametri mbox nella richiesta mbox globale.

Supporta la raccolta di dati da più provider tramite richieste sincrone e asincrone.

L&#39;utilizzo di questo approccio semplifica la gestione della visualizzazione momentanea del contenuto della pagina predefinito, inclusi i timeout indipendenti per ogni provider per limitare l&#39;impatto sulle prestazioni della pagina.

## Avvertenze

Se i provider di dati aggiunti a `window.targetGlobalSettings.dataProviders` sono asincroni, vengono eseguiti in parallelo. La richiesta API dei visitatori viene eseguita in parallelo con le funzioni aggiunte a `window.targetGlobalSettings.dataProviders` per consentire un tempo minimo di attesa.

at.js non tenta di memorizzare i dati nella cache. Se il fornitore di dati recupera i dati una sola volta, deve assicurarsi che i dati siano memorizzati nella cache e, quando viene invocata la funzione di fornitore di dati, deve servire i dati della cache per la seconda invocazione.

## Esempi di codice

Diversi esempi sono disponibili in [Fornitori di dati](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

## Collegamenti alle informazioni rilevanti

Documentazione: [Fornitori di dati](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)

## Video di formazione:

* [Utilizzo di Fornitori di dati in Adobe Target](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html)
* [Implementazione di Fornitori di dati in Adobe Target](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html)
