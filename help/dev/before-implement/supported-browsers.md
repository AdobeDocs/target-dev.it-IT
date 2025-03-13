---
keywords: Browser, prerequisiti, requisiti, Internet Explorer, Chrome, Firefox, Safari, Android, Surface, Browser0
description: Scopri quali browser Internet [!DNL Adobe Target] supportano per la relativa interfaccia e per la distribuzione dei contenuti.
title: Quali Browser Sono Supportati Da  [!DNL Target] ?
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
source-git-commit: 1b6dcb24d677b758ed1daf85dc0a7e9e5b42680d
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 20%

---

# Browser supportati

L’applicazione [!DNL Adobe Target] e la consegna dei contenuti sono stati testati su una vasta gamma di browser e dispositivi.

Per ulteriori informazioni importanti su TLS, vedere [Modifiche alla crittografia di TLS (Transport Layer Security)](tls-transport-layer-security-encryption.md).

## Interfaccia di [!DNL Target] Standard/Premium

L&#39;interfaccia [!DNL Target] supporta i seguenti browser e dispositivi:

>[!NOTE]
>
>Target supporta la versione più recente di ciascun browser elencato e la versione più recente meno 1.


| Tipo di dispositivo | Versione del browser |
|--- |--- |
| [!DNL Windows] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |
| [!DNL Mac] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |

## Requisiti di editing video

Per poter aprire, creare e visualizzare in anteprima le pagine Web in modo affidabile nel [!UICONTROL Visual Experience Composer] (Compositore esperienza visivo), è necessario che nel browser Web sia installata l&#39;[estensione del browser Helper per editing video di Adobe Experience Cloud](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank} oppure utilizzare [!UICONTROL Enhanced Experience Composer (EEC)].

>[!NOTE]
>
>[!DNL Google Chrome] e [!DNL Microsoft Edge] sono attualmente gli unici browser che supportano la modifica visiva delle pagine Web in [!DNL Adobe Target].


## Consegna dei contenuti

La consegna dei contenuti è stata testata attraverso i seguenti browser e dispositivi:

| Tipo di dispositivo | Versione del browser |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9 e 10. Testato in modalità emulazione. **Nota**: la distribuzione dei contenuti in IE 9 non è più supportata con at.js 1.3.0 (e versioni successive). La distribuzione dei contenuti su IE 10, 11 e tutte le versioni precedenti non è più supportata con at.js 2.5.0 (e versioni successive).</li><li>Internet Explorer 11. **Nota**: la distribuzione dei contenuti in IE 10, 11 e in tutte le versioni precedenti non è più supportata con at.js 2.5.0 (e versioni successive).</li><li>Microsoft Edge</li><li>Chrome (più recente meno 1)</li><li>Firefox (più recente meno 1)</li></ul> |
| Mac | <ul><li>Apple Safari (Più Recente). **Nota**: per ulteriori informazioni su come Safari gestisce i cookie di prima e terze parti, consulta [Cookie di Target](../implement/client-side/atjs/atjs-cookies.md).</li><li>Firefox (più recente meno 1)</li><li>Chrome (più recente meno 1)</li></ul> |
| Cellulare/Tablet | <ul><li>Apple iOS (più recente)</li><li>Dispositivi Android e Tablet (Android 4 e versioni successive)</li><li>Microsoft Surface (Windows 8.1)</li></ul> |

Tieni presente quanto segue:

* [!DNL Adobe Experience Platform Web SDK] è progettato per funzionare in modo ottimale nelle versioni più recenti di [!DNL Google Chrome], [!DNL Safari], [!DNL Firefox] e [!DNL Microsoft Edge Chromium]. È possibile che si verifichino problemi durante l&#39;utilizzo di alcune funzionalità nelle versioni precedenti di questi browser o nei browser obsoleti, ad esempio [!DNL Internet Explorer].
* Per le implementazioni at.js, [!DNL Target] visualizza il contenuto predefinito nelle versioni precedenti di Internet Explorer e possibilmente nelle versioni precedenti dei browser sopra elencati.
* In Internet Explorer tutti gli elementi sconosciuti, ad esempio gli elementi personalizzati, vengono trattati come lo stesso tipo di elemento. Di conseguenza, la consegna non funziona con gli elementi personalizzati.
* [!DNL Target] visualizza il contenuto predefinito nei browser non elencati sopra e nei browser che utilizzano la [modalità quirks](https://en.wikipedia.org/wiki/Quirks_mode). at.js richiede un doctype che esegua il rendering in modalità standard, ad esempio: `<!DOCTYPE html>`.
* L&#39;infrastruttura di consegna [!DNL Adobe] è stata protetta per NON supportare dispositivi e browser TLS 1.0 dopo il 12 settembre 2018. Consulta [Modifiche alla crittografia di TLS (Transport Layer Security)](../before-implement/tls-transport-layer-security-encryption.md) per capire l’impatto complessivo di questa modifica.
