---
keywords: registerExtension, registerextension, register extension, at.js, funzioni, funzione, clientCode, serverDomain, globalMboxName, globalMboxAutoCreate, timeout, registerExtension2
description: Utilizza la funzione [!UICONTROL registerExtension()] per la libreria JavaScript at.js  [!DNL Adobe Target]  per registrare un'estensione specifica. (at.js 1.x)
title: Come si utilizza la funzione [!UICONTROL registerExtension()]?
feature: at.js
exl-id: 71decf00-84c5-4914-b0cd-bb061fa6265f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 69%

---

# [!UICONTROL registerExtension()] - at.js 1.x

Fornisce un metodo standard per registrare un’estensione specifica.

>[!NOTE]
>
>Questa funzione è disponibile per at.js versione 1.Solo *x*. Questa funzione è stata rimossa con il rilascio di at.js 2.*x*. Questa funzione restituisce il contenuto predefinito se utilizzata con at.js 2.x.

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
