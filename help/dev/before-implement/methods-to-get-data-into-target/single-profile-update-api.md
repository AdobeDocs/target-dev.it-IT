---
keywords: implementare, implementare, configurare, configurare, aggiornare un singolo profilo
description: Inserire dati in [!DNL Target] utilizzando l’API di aggiornamento a profilo singolo.
title: Come posso inserire dati in [!DNL Target] Utilizzare l’API di aggiornamento a profilo singolo?
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 734bda64915a08f2edba37cbbb66b2de581c2237
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 15%

---

# API di aggiornamento a profilo singolo

Quasi identico al [!UICONTROL API di aggiornamento del profilo bulk] in [!DNL Adobe Target], ma un profilo visitatore viene aggiornato alla volta, in linea nella chiamata API invece che con un file .csv.

## Formato

Il visitatore deve essere identificato tramite [!DNL Target] `mboxPC` valore o `mbox3rdPartyId` valore. Il [!UICONTROL ID EXPERIENCE CLOUD] (ECID) non è supportato.

## Casi d’uso di esempio

Desideri aggiornare il profilo di un singolo visitatore che esegue un’azione offline. Le azioni possono includere il raggiungimento di un call center, il finanziamento di un prestito, l’utilizzo di una carta fedeltà in negozio, l’accesso a un chiosco e così via.

## Vantaggi del metodo

* Nessun limite al numero di attributi del profilo.*
* Gli attributi del profilo inviati tramite il sito possono essere aggiornati tramite l’API e in modo opposto.

## Avvertenze

* Limite delle chiamate API di 1 milione per 24 ore.
* Aggiorna solo i profili. Impossibile creare un profilo per un utente potenziale [!DNL Target] deve ancora vedere.
* Gli aggiornamenti in genere si verificano in meno di un’ora, ma la visualizzazione potrebbe richiedere fino a 24 ore.

## Esempi di codice

GET e POST supportati.

```
https://CLIENT.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr1=0&profile.attr2=1...
```

## Risorse

* [Aggiornamento di profili](https://developers.adobetarget.com/api/#updating-profiles)
