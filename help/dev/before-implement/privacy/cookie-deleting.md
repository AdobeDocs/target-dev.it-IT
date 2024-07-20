---
keywords: cookie, cookie, elimina cookie, elimina [!DNL Target] cookie, google chrome, chrome, mozilla firefox, firefox, microsoft edge, safari, cookie1
description: Scopri come eliminare i cookie del browser  [!DNL Target]  per convalidare le esperienze.
title: Come si elimina il cookie  [!DNL Target] ?
feature: Privacy & Security
exl-id: c975c47f-8d81-4abe-aa89-f65275a73002
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '355'
ht-degree: 2%

---

# Elimina il cookie [!DNL Target]

Puoi eliminare il cookie del browser [!DNL Adobe Target] (mbox) in modo da convalidare tutte le esperienze durante il test.

Se non è presente alcun cookie [!DNL Target] (mbox), l&#39;utente viene considerato un nuovo visitatore e le viene mostrata una nuova esperienza. Esistono diversi modi per eliminare la mbox senza eliminare tutti i cookie del browser.

>[!NOTE]
>
>Le seguenti istruzioni sono corrette per i browser e le versioni elencati. Cerca in Internet le istruzioni per il browser o la versione specifici.

## Elimina il cookie [!DNL Target] da Google Chrome

Versione 84.0.4147,105

1. Fare clic sul menu **[!UICONTROL Chrome]** > **[!UICONTROL Preferences]**.
1. Fare clic sulla scheda **[!UICONTROL Privacy and Security]**.
1. Fare clic su **[!UICONTROL Cookies and other site data]**.
1. Fare clic su **[!UICONTROL See all cookies and site data]**.
1. Espandi la sezione `adobe.com`, seleziona il cookie **mbox**, quindi fai clic sull&#39;icona Elimina (X).

## Elimina il cookie [!DNL Target] da Mozilla Firefox

Versione 79.0

### Elimina tutti i cookie associati a `adobe.com`

1. Fare clic sul menu **[!UICONTROL Firefox]** > **[!UICONTROL Preferences]**.
1. Fare clic sulla scheda **[!UICONTROL Privacy and Security]**.
1. In **Cookie e dati del sito*, fare clic su **[!UICONTROL Manage Data]**.
1. Selezionare il sito `adobe.com`, quindi fare clic su **[!UICONTROL Remove Selected]**.

>[!WARNING]
>
>Verranno eliminati tutti i cookie associati al sito `adobe.com`. Se desideri eliminare un singolo cookie per un sito, segui le istruzioni riportate di seguito.

### Eliminare un singolo cookie (mbox)

1. In Firefo, fare clic su **[!UICONTROL Tools]** > **[!UICONTROL Web Developer]** > **[!UICONTROL Storage Inspector]**.
1. Fare clic sulla scheda **[!UICONTROL Advanced]**.
1. Passa alla pagina Web contenente il cookie che desideri eliminare.
1. Espandere la sezione **[!UICONTROL Cookies]**, quindi fare clic su `https://experience.adobe.com`.
1. Fare clic con il pulsante destro del mouse sul cookie **[!UICONTROL mbox]**, quindi scegliere **[!UICONTROL Delete]**.

## Elimina il cookie [!DNL Target] da Microsoft Edge

Versione 84.0.522.52

1. Fare clic sul menu **[!UICONTROL Microsoft Edge]** > **[!UICONTROL Preferences]**.
1. Fare clic sulla scheda **[!UICONTROL Site Permissions]**.
1. Fare clic su **[!UICONTROL Cookies and site data]**.
1. Fare clic su **[!UICONTROL See all cookies and site data]**.
1. Espandi la sezione `adobe.com`, seleziona il cookie **mbox**, quindi fai clic sull&#39;icona Elimina (X).

## Elimina il cookie [!DNL Target] da Apple Safari

Versione 13.1.2

### Elimina tutti i cookie associati a `adobe.com`

1. Fare clic sul menu **[!UICONTROL Safari]** > **[!UICONTROL Preferences]**.
1. Fare clic sulla scheda **[!UICONTROL Privacy]**.
1. Fare clic su **[!UICONTROL Manage Website Data]**.
1. Selezionare i siti per i cookie da eliminare, quindi fare clic su **[!UICONTROL Remove]**.

>[!WARNING]
>
>Verranno eliminati tutti i cookie associati al sito `adobe.com`. Se desideri eliminare un singolo cookie per un sito, segui le istruzioni riportate di seguito.

### Eliminare un singolo cookie (mbox)

1. Fare clic sul menu **[!UICONTROL Safari]** > **[!UICONTROL Preferences]**.
1. Fare clic sulla scheda **[!UICONTROL Advanced]**.
1. Selezionare l&#39;opzione **[!UICONTROL Show Develop menu in menu bar]**.
1. Passa alla pagina Web contenente il cookie che desideri eliminare.
1. Fare clic sul menu **[!UICONTROL Develop]** > **[!UICONTROL Show Web Inspector]**.
1. Fare clic sulla scheda **[!UICONTROL Storage]**.
1. Espandere la sezione **[!UICONTROL Cookies]**, quindi fare clic su `www.adobe.com`.
1. Fare clic con il pulsante destro del mouse sul cookie **mbox**, quindi scegliere **[!UICONTROL Delete]**.
