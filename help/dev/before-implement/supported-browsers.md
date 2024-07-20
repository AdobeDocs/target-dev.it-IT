---
keywords: Browser, prerequisiti, requisiti, Internet Explorer, Chrome, Firefox, Safari, Android, Surface, Browser0
description: Scopri quali browser Internet [!DNL Adobe Target] supportano per la relativa interfaccia e per la distribuzione dei contenuti.
title: Quali Browser Sono Supportati Da  [!DNL Target] ?
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 26%

---

# Browser supportati

L’applicazione [!DNL Adobe Target] e la consegna dei contenuti sono stati testati su una vasta gamma di browser e dispositivi.

Per ulteriori informazioni importanti su TLS, vedere [Modifiche alla crittografia di TLS (Transport Layer Security)](tls-transport-layer-security-encryption.md).

## Interfaccia di [!DNL Target] Standard/Premium

L&#39;interfaccia [!DNL Target] supporta i seguenti browser e dispositivi:

| Tipo di dispositivo | Versione del browser |
|--- |--- |
| Windows | <ul><li>Microsoft Edge</li><li>Google Chrome (più recente meno 1)</li><li>Mozilla Firefox (più recente, meno recente 1)</li></ul> |
| Mac | <ul><li>Firefox (più recente meno 1)</li><li>Chrome (più recente meno 1)</li></ul> |

## Consegna dei contenuti

La consegna dei contenuti è stata testata attraverso i seguenti browser e dispositivi:

| Tipo di dispositivo | Versione del browser |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9 e 10. Testato in modalità emulazione. **Nota**: la distribuzione dei contenuti in IE 9 non è più supportata con at.js 1.3.0 (e versioni successive). La distribuzione dei contenuti su IE 10, 11 e tutte le versioni precedenti non è più supportata con at.js 2.5.0 (e versioni successive).</li><li>Internet Explorer 11. **Nota**: la distribuzione dei contenuti in IE 10, 11 e in tutte le versioni precedenti non è più supportata con at.js 2.5.0 (e versioni successive).</li><li>Microsoft Edge</li><li>Chrome (più recente meno 1)</li><li>Firefox (più recente meno 1)</li></ul> |
| Mac | <ul><li>Apple Safari (Più Recente). **Nota**: per ulteriori informazioni su come Safari gestisce i cookie di prima e terze parti, consulta [Cookie di Target](../implement/client-side/atjs/atjs-cookies.md).</li><li>Firefox (più recente meno 1)</li><li>Chrome (più recente meno 1)</li></ul> |
| Cellulare/Tablet | <ul><li>Apple iOS (più recente)</li><li>Dispositivi Android e Tablet (Android 4 e versioni successive)</li><li>Microsoft Surface (Windows 8.1)</li></ul> |

Tieni presente quanto segue:

* Per le implementazioni at.js, [!DNL Target] visualizza il contenuto predefinito nelle versioni precedenti di Internet Explorer e possibilmente nelle versioni precedenti dei browser sopra elencati.
* In Internet Explorer tutti gli elementi sconosciuti, ad esempio gli elementi personalizzati, vengono trattati come lo stesso tipo di elemento. Di conseguenza, la consegna non funziona con gli elementi personalizzati.
* [!DNL Target] visualizza il contenuto predefinito nei browser non elencati sopra e nei browser che utilizzano la [modalità quirks](https://en.wikipedia.org/wiki/Quirks_mode). at.js richiede un doctype che esegua il rendering in modalità standard, ad esempio: `<!DOCTYPE html>`.
* L&#39;infrastruttura di consegna [!DNL Adobe] è stata protetta per NON supportare dispositivi e browser TLS 1.0 dopo il 12 settembre 2018. Consulta [Modifiche alla crittografia di TLS (Transport Layer Security)](../before-implement/tls-transport-layer-security-encryption.md) per capire l’impatto complessivo di questa modifica.
