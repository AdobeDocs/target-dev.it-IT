---
keywords: implementare, implementare, configurare, impostare gli attributi del profilo
description: Ottieni dati in [!DNL Target] utilizzando gli attributi del profilo di script.
title: Come posso inserire dati in [!DNL Target] utilizzando gli attributi del profilo di script?
feature: Implementation
exl-id: ba11f1de-e68b-4505-8e3e-cd4d46ef59a2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 74%

---

# Attributi di profilo script

Gli attributi del profilo di script sono coppie nome/valore definite nella soluzione [!DNL Adobe Target]. Il valore è determinato dall&#39;esecuzione di un frammento JavaScript sul server di destinazione per ogni chiamata del server.

Gli utenti scrivono frammenti di codice di piccole dimensioni che vengono eseguiti per chiamata mbox e prima che un visitatore venga valutato per l&#39;appartenenza al pubblico e all&#39;attività.

## Formato

Gli attributi di profilo script vengono creati nella sezione Tipi di pubblico di Target. Qualsiasi nome di attributo è valido e il valore è il risultato di una funzione JavaScript scritta dall&#39;utente [!DNL Target]. Il nome dell&#39;attributo viene automaticamente preceduto da “user. &quot; in [!DNL Target] per distinguerli dagli attributi di profilo nella pagina.

Il frammento di codice è scritto nel linguaggio Rhino JS e può fare riferimento a token e altri valori.

## Esempi di casi d&#39;uso

* **Abbandono del carrello**: quando il visitatore raggiunge il carrello, imposta lo script del profilo su 1. Quando il visitatore si converte, reimpostarlo a 0. Se il valore è uguale a 1, allora il visitatore ha un elemento nel carrello.
* **Conteggio delle visite**: a ogni nuova visita, incrementa il conteggio di 1 per tenere traccia di quanto spesso un visitatore ritorna al sito.

## Vantaggi del metodo

Non richiede aggiornamenti del codice della pagina.

Viene eseguito prima delle decisioni relative all&#39;appartenenza al pubblico e all&#39;attività, pertanto questi attributi di script profilo possono influenzare l&#39;appartenenza a una singola chiamata al server.

Può essere molto robusto. Ben 2000 istruzioni possono essere eseguite per ogni script.

## Avvertenze

Richiede conoscenza di JavaScript.

L&#39;ordine di esecuzione degli script di profilo non può essere garantito, quindi non possono contare l&#39;uno sull&#39;altro.

Il debug può essere difficile da eseguire.

## Esempi di codice

Gli script di profilo sono abbastanza flessibili:

```
user.purchase_recency: var dayInMillis = 3600 * 24 * 1000; if (mbox.name == 'orderThankyouPage') {  user.setLocal('lastPurchaseTime', new Date().getTime()); } var lastPurchaseTime = user.getLocal('lastPurchaseTime'); if (lastPurchaseTime) {  return ((new Date()).getTime()-lastPurchaseTime)/dayInMillis; }
```

### Collegamenti alle informazioni rilevanti

[Attributi degli script di profilo](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html#concept_8C07AEAB0A144FECA8B4FEB091AED4D2)
