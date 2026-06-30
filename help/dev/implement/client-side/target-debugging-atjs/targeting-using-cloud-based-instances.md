---
keywords: istanze cloud, elenco suffissi pubblici, suffisso pubblico, cookie, cookie di prime parti, cookie di prime parti, azurewebsites.net, cloudapp.net, amazonaws.com, cloudfront.net, herokuapp.com firebaseapp.com, targetGlobalSettings, cookieDomain, istanze cloud5, istanze cloud6, istanze cloud7, istanze cloud8, istanze cloud9, istanze suffisso pubblico list0, suffisso pubblico list1, suffisso pubblico list2, suffisso pubblico list3, suffisso pubblico list4, suffisso pubblico list5
description: Esplora i problemi (con le soluzioni) riscontrati dai clienti quando utilizzano istanze basate su cloud per testare [!DNL Adobe Target]  o a scopo di verifica.
title: Posso usare  [!DNL Target]  con istanze basate su cloud?
feature: at.js
exl-id: 4b24fdc0-6c74-4b29-bbf9-7a761d4564a2
TQID: https://experienceleague.adobe.com/df63sTQxukCfa4pYc1X6FRvxV3cY2UG-ixk5v9Fqh-c
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d851e2344279caeae25e4823ca86b9c17efd63
workflow-type: tm+mt
source-wordcount: 203
ht-degree: 46%

---

# Usa istanze basate su cloud con [!DNL Target]

Informazioni sui problemi riscontrati dai clienti quando si utilizzano istanze basate su cloud per testare [!DNL Adobe Target].

I [!DNL Target] clienti di utilizzano talvolta istanze basate su cloud con [!DNL Target] per test o semplici prove di concetto. Queste istanze possono includere i seguenti domini:

`azurewebsites.net`, `cloudapp.net`, `amazonaws.com`, `cloudfront.net`, `herokuapp.com` o `firebaseapp.com`

Questi domini, e molti altri, fanno parte dell’[elenco dei suffissi pubblici](https://publicsuffix.org/list/public_suffix_list.dat).

**Problema:** se si utilizzano questi domini, i browser moderni non salvano i cookie.

La libreria JavaScript at.js utilizza i cookie per tenere traccia degli utenti in modo che [!DNL [!DNL Target]] fornisca sempre un&#39;esperienza coerente. Se la libreria JavaScript [!DNL Target] non è in grado di salvare i cookie, le richieste di Target sono disabilitate.

**Soluzione:** come best practice, se vuoi utilizzare istanze basate su cloud con domini inclusi nellʼelenco dei suffissi pubblici, personalizza lʼimpostazione `cookieDomain`. Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).



