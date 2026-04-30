---
keywords: istanze cloud, elenco suffissi pubblici, suffisso pubblico, cookie, cookie di prime parti, cookie di prime parti, azurewebsites.net, cloudapp.net, amazonaws.com, cloudfront.net, herokuapp.com firebaseapp.com, targetGlobalSettings, cookieDomain, istanze cloud5, istanze cloud6, istanze cloud7, istanze cloud8, istanze cloud9, istanze suffisso pubblico list0, suffisso pubblico list1, suffisso pubblico list2, suffisso pubblico list3, suffisso pubblico list4, suffisso pubblico list5
description: Esplora i problemi (con le soluzioni) riscontrati dai clienti quando utilizzano istanze basate su cloud per testare [!DNL Adobe Target]  o a scopo di verifica.
title: Posso usare  [!DNL Target]  con istanze basate su cloud?
feature: at.js
exl-id: 4b24fdc0-6c74-4b29-bbf9-7a761d4564a2
source-git-commit: a85cbaafb3c666478dd4d283661ff4b1125aa0dd
workflow-type: tm+mt
source-wordcount: '203'
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

