---
keywords: mbox globale, implementa at.js
description: Scopri la mbox globale in [!DNL Adobe Target], a name used to refer to the single server call made at the top of each web page in your [!DNL Target] implementazione.
title: Cos'è una mbox globale?
feature: at.js
exl-id: 572c1dc6-5cdd-427a-9458-e5ec49990cf8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 73%

---

# Comprendere la mbox globale

Informazioni sulla mbox globale, ossia la singola chiamata del server effettuata nella parte superiore di ogni pagina Web nell’implementazione [!DNL Adobe Target].

Per impostazione predefinita, la mbox globale è denominata `target-global-mbox`. Se necessario, puoi rinominarla per il tuo account.

Ci sono varie differenze tra una mbox normale (non globale) e la mbox globale, tra cui:

| Mbox normale | Mbox globale |
|--- |--- |
| In una mbox normale in genere i contenuti sono racchiusi con un tag `<DIV>`. | La mbox globale è “vuota” e non racchiude alcun contenuto. |
| Il contenuto di una sola attività può essere fornito in una mbox normale. | Il contenuto di più attività può essere fornito in una risposta a una mbox globale. |

Se si forniscono più attività tramite la mbox globale o tramite più mbox normali, Target [determina la priorità](https://experienceleague.adobe.com/docs/target/using/activities/priority.html) tramite la quale l’attività (o le attività) viene consegnata a una pagina web.

Dati aggiuntivi a livello di pagina possono essere inviati a [!DNL Target] insieme alla mbox globale utilizzando la funzione `[!UICONTROL targetPageParams]`. Questa procedura è simile alla funzionalità del parametro mbox. Per ulteriori informazioni, consulta [Trasmissione di parametri a una mbox globale](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md).
