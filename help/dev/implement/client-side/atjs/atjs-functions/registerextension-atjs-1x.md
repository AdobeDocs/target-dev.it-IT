---
keywords: registerExtension, registerextension, register extension, at.js, funzioni, funzione, clientCode, serverDomain, globalMboxName, globalMboxAutoCreate, timeout, registerExtension2
description: Utilizza la funzione [!UICONTROL registerExtension()] per la libreria JavaScript at.js  [!DNL Adobe Target]  per registrare un'estensione specifica. (at.js 1.x)
title: Come si utilizza la funzione [!UICONTROL registerExtension()]?
feature: at.js
exl-id: 71decf00-84c5-4914-b0cd-bb061fa6265f
TQID: https://experienceleague.adobe.com/qTWubp0dNesN-8vsooz8pdbjfSw1W1ktm-0bG6YRzJw
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 274
ht-degree: 63%

---

# [!UICONTROL registerExtension()] - at.js 1.x

Fornisce un metodo standard per registrare un’estensione specifica.

>[!NOTE]
>
>Questa funzione è disponibile solo per at.js versione 1.*x*. Questa funzione è stata rimossa con il rilascio di at.js 2.*x*. Questa funzione restituisce il contenuto predefinito se utilizzata con at.js 2.x.

Il parametro delle opzioni è obbligatorio e ha la seguente struttura:

| Chiave | Tipo | Obbligatorio | Descrizione |
|--- |--- |--- |--- |
| name | Stringa | Sì | Nome di estensione. |
| modules | Array[Stringa] | Sì | Array di stringhe che rappresentano i nomi dei moduli richiesti. |
| register | Funzione | Sì | Una funzione utilizzata per inizializzare e compilare l&#39;estensione. Questa funzione riceve argomenti basati sull’array dei moduli. |

Note:

* se uno dei parametri non viene fornito, viene generata un&#39;eccezione.
* Se l’array dei moduli è vuoto, viene generata un&#39;eccezione.

Per ulteriori informazioni ed esempi sull&#39;utilizzo di `[!UICONTROL registerExtension]`, vedere la pagina [Estensioni atjs di Adobe Experience Cloud Target](https://github.com/Adobe-Marketing-Cloud/target-atjs-extensions) su GitHub.

## Metodi del modulo impostazioni

| Chiave | Tipo | Descrizione |
|--- |--- |--- |
| clientCode | Stringa | Codice cliente |
| serverDomain | Stringa | Dominio server Edge |
| globalMboxName | Stringa | Nome mbox globale [!DNL Target] |
| globalMboxAutoCreate | Booleano | Indica se la creazione automatica è abilitata o meno |
| timeout | Numero | Timeout richiesta |

## Metodi del modulo logger

| Chiave | Tipo | Descrizione |
|--- |--- |--- |
| log | Funzione | Registra l&#39;elenco di variabili di argomenti nella console del browser, se esiste. Viene attivato solo quando `mboxDebug=true` viene passato all&#39;URL. |
| error | Funzione | Registra l&#39;elenco delle variabili degli argomenti nella console del browser. Viene attivato solo quando sono presenti errori gravi, ad esempio timeout di rete, nodo HTML non trovato, ecc. |
