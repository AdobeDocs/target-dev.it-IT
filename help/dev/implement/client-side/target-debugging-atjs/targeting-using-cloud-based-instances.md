---
keywords: istanze cloud, elenco suffissi pubblici, suffisso pubblico, cookie, cookie di prime parti, cookie di prime parti, azurewebsites.net, cloudapp.net, amazonaws.com, cloudfront.net, herokuapp.com firebaseapp.com, targetGlobalSettings, cookieDomain, istanze cloud5, istanze cloud6, istanze cloud7, istanze cloud8, istanze cloud9, istanze suffisso pubblico list0, suffisso pubblico list1, suffisso pubblico list2, suffisso pubblico list3, suffisso pubblico list4, suffisso pubblico list5
description: Esplora i problemi (con le soluzioni) riscontrati dai clienti quando si utilizzano istanze basate su cloud per testare [!DNL Adobe Target] o a scopo di verifica di concetto.
title: Posso Utilizzare [!DNL Target] con istanze basate su cloud?
feature: at.js
exl-id: 4b24fdc0-6c74-4b29-bbf9-7a761d4564a2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 49%

---

# Utilizzare istanze basate su cloud con [!DNL Target]

Informazioni sui problemi riscontrati dai clienti quando si utilizzano istanze basate su cloud per testare [!DNL Adobe Target].

I [!DNL Target] clienti di utilizzano talvolta istanze basate su cloud con [!DNL Target] per test o semplici prove di concetto. Queste istanze possono includere i seguenti domini:

`azurewebsites.net`, `cloudapp.net`, `amazonaws.com`, `cloudfront.net`, `herokuapp.com` o `firebaseapp.com`

Questi domini, e molti altri, fanno parte dell’[elenco dei suffissi pubblici](https://publicsuffix.org/list/public_suffix_list.dat).

**Problema:** se si utilizzano questi domini, i browser moderni non salvano i cookie.

La libreria JavaScript at.js utilizza i cookie per monitorare gli utenti in modo che [!DNL [!DNL Target]] presenta sempre un’esperienza coerente. Se il [!DNL Target] La libreria JavaScript non può salvare i cookie, le richieste Target sono disabilitate.

**Soluzione:** come best practice, se vuoi utilizzare istanze basate su cloud con domini inclusi nellʼelenco dei suffissi pubblici, personalizza lʼimpostazione `cookieDomain`. Per ulteriori informazioni, consulta [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).
