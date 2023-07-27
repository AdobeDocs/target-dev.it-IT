---
keywords: risoluzione problemi, domande frequenti, FAQ, domande e risposte, globale, mbox globale
description: Leggi le domande frequenti e le risposte sull’Adobe [!DNL Target] mbox globali.
title: Quali sono le domande frequenti sulla mbox globale?
feature: at.js
exl-id: 7bcd1b67-809a-466a-b648-6e0e44386157
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 63%

---

# Domande frequenti sulla mbox globale

Elenco delle domande frequenti sulle mbox globali.

## Posso avere più di una mbox globale se il mio [!DNL Target] l’account è impostato su più domini?

L&#39;account supporta un&#39;unica mbox globale.

Puoi definire un limite per l&#39;esecuzione delle attività aggiungendo a queste delle regole URL. Per ulteriori informazioni, consulta [Includere la stessa esperienza in pagine simili](https://experienceleague.adobe.com/docs/target/using/experiences/vec/temtest.html).

Puoi anche trasmettere un parametro sulla pagina utilizzando [targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) e quindi selezionare tali parametri nella sezione &quot;Configura URL&quot; nel [!UICONTROL Compositore esperienza visivo][!UICONTROL  o aggiungendo i parametri come “perfezionamenti” nel Compositore esperienza basato su modulo].

## Come faccio a trasferire i dati dei ricavi a [!DNL Target] mbox globale?

Per raccogliere informazioni su ricavi e ordini su target-global-mbox, è necessario inviare i &quot;parametri mbox&quot; a [!DNL Target]. Questi parametri sono coppie nome/valore utilizzate per inviare ulteriori informazioni a [!DNL Target]. [!DNL Target]In viene eseguita la ricerca automatica di tali parametri (nomi riservati) allo scopo di popolare i dati dei ricavi.

Per `orderConfirmPage`, è necessario trasmettere `orderTotal`, `orderId` e `productPurchasedId`.

Questi parametri devono essere inviati a target-global-mbox tramite `targetPageParams()`. Per ulteriori informazioni, consulta [Trasmissione di parametri a una mbox globale](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md).

Puoi anche aggiungere il targeting alla parte di conversione in modo che [!DNL Target] conta le conversioni solo su target-global-mbox quando viene visualizzata la pagina di conferma dell’ordine, come illustrato di seguito:

![immagine alt](assets/revenue1.png)

Nella sezione Pagine del sito sopra illustrata sono incluse le seguenti selezioni: Pagina corrente, URL, contiene, orderconfirm.

![immagine alt](assets/revenue2.png)

Nelle opzioni dell’illustrazione precedente sono incluse le seguenti impostazioni:

* **Cosa desideri misurare con questa attività:** Ricavi.
* **Vista predefinita per rapporti:** Ricavo per visitatore (RPV).
* **Qual è l&#39;azione intrapresa dal pubblico per indicare che l&#39;obiettivo è stato raggiunto?** Visualizzazione di una Mbox, target-global-mbox
