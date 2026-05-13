---
keywords: risoluzione problemi, domande frequenti, FAQ, domande e risposte, globale, mbox globale
description: Leggi le domande frequenti e le risposte su Adobe [!DNL Target] mbox globali.
title: Quali sono le domande frequenti sulla mbox globale?
feature: at.js
exl-id: 7bcd1b67-809a-466a-b648-6e0e44386157
TQID: https://experienceleague.adobe.com/bxsjCqSQpp6M20StzZtMBrfxjJCKgPEPfS2OlBUP00A
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 309
ht-degree: 32%

---

# Domande frequenti sulla mbox globale

Elenco delle domande frequenti sulle mbox globali.

## Posso avere più di una mbox globale se il mio account [!DNL Target] è impostato su più domini?

L&#39;account supporta un&#39;unica mbox globale.

Puoi definire un limite per l&#39;esecuzione delle attività aggiungendo a queste delle regole URL. Per ulteriori informazioni, vedere [Includere la stessa esperienza in pagine simili](https://experienceleague.adobe.com/docs/target/using/experiences/vec/temtest.html).

Puoi anche trasmettere un parametro sulla pagina utilizzando [targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) e quindi selezionare tali parametri nella sezione &quot;Configura URL&quot; nel [!UICONTROL Visual Experience Composer] (VEC) o aggiungendo i parametri come &quot;perfezionamenti&quot; in [!UICONTROL Form-Based Experience Composer].

## Come si trasmettono i dati sui ricavi a una mbox globale [!DNL Target]?

Per raccogliere informazioni sui ricavi e sugli ordini in target-global-mbox, è necessario inviare i &quot;parametri mbox&quot; a [!DNL Target]. Questi parametri sono coppie nome/valore utilizzate per inviare ulteriori informazioni a [!DNL Target]. [!DNL Target] cerca automaticamente questi parametri (nomi riservati) per compilare i dati dei ricavi.

Per `orderConfirmPage`, è necessario passare `orderTotal`, `orderId` e `productPurchasedId`.

Questi parametri devono essere inviati a target-global-mbox tramite `targetPageParams()`. Per ulteriori informazioni, consulta [Trasmissione di parametri a una mbox globale](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md).

Aggiungere inoltre il targeting alla parte di conversione in modo che [!DNL Target] conteggi le conversioni solo su target-global-mbox quando viene visualizzata la pagina di conferma dell&#39;ordine, come illustrato di seguito:

![Alt immagine](assets/revenue1.png)

Nella sezione Pagine del sito sopra illustrata sono incluse le seguenti selezioni: Pagina corrente, URL, contiene, orderconfirm.

![Alt immagine](assets/revenue2.png)

Nelle opzioni dell’illustrazione precedente sono incluse le seguenti impostazioni:

* **Cosa desideri misurare con questa attività:** Ricavi.
* **Vista predefinita per rapporti:** Ricavo per visitatore (RPV).
* **Qual è l&#39;azione intrapresa dal pubblico per indicare che l&#39;obiettivo è stato raggiunto?** Visualizzazione di una mbox, target-global-mbox
