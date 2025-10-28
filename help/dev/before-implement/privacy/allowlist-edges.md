---
keywords: implementa, implementa, whitelist, whitelist, inserisce nell'elenco Consentiti di, elenco consentiti, edge, edge, 9 $
description: Visualizza un elenco di host per aiutarti a inserire nell'elenco Consentiti [!DNL Adobe Target] gli spigoli (nodi di servizio geograficamente distribuiti che garantiscono tempi di risposta ottimali agli utenti finali).
title: Inserire nell'elenco Consentiti Come posso  [!DNL Target] nodi Edge?
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 662d415bc3c216bcd038f07dcaa0fd83f6518690
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Inserire nell&#39;elenco Consentiti [!DNL Target] nodi edge

Informazioni e un elenco aggiornato di host per aiutarti a inserire nell&#39;elenco Consentiti i bordi di [!DNL Adobe Target] di cui hai bisogno.

Un perimetro è un’architettura di servizio geograficamente distribuita che garantisce agli utenti finali che richiedono contenuti tempi di risposta ottimali, indipendentemente dall’area geografica in cui si trovano. Ogni nodo perimetrale dispone di tutte le informazioni necessarie per rispondere alla richiesta di contenuto dell’utente e per tenere traccia dei dati di analisi relativi a tale richiesta. Le richieste degli utenti vengono indirizzate al nodo edge più vicino. Per ulteriori informazioni, vedere [La rete Edge](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934).

Inserire nell&#39;elenco Consentiti Se necessario, puoi [!DNL Target] nodi edge.

>[!IMPORTANT]
>
>Oltre a inserire nell&#39;elenco Consentiti gli indirizzi IP NAT (Network Address Translation) di [!DNL Target] edge e [!DNL Target] indirizzi IP edge di cui all&#39;articolo, è necessario anche inserire nell&#39;elenco Consentiti tutti i blocchi di indirizzi IP di [!DNL Adobe Analytics].
>
>Per ulteriori informazioni, consulta [Tutti i blocchi di indirizzi IP di Adobe Analytics](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank} nella *note tecniche di Adobe Analytics* documentazione.
>
>È in corso l&#39;aggiornamento dell&#39;infrastruttura [!DNL Adobe Target] e i clienti che desiderano inserire nell&#39;elenco Consentiti gli indirizzi devono utilizzare entrambi i set di IP. In caso contrario, i clienti che utilizzano implementazioni lato server o ibride in cui le chiamate API di Target per il recupero delle esperienze hanno origine dall’interno di una rete dietro un firewall configurato per l’utilizzo di un elenco Consentiti di.

Per garantire un accesso ininterrotto a [!DNL Target] tramite [!DNL Experience Edge Connector], i clienti possono aggiornare le configurazioni di rete per consentire il traffico verso il servizio proxy.

## Panoramica del servizio proxy

* **Endpoint servizio**: `https://tnt-web-proxy.adobe.io`.
* **Infrastruttura**: ospitata sulla piattaforma Ethos [!DNL Adobe].
* **Nota**: questo servizio utilizza il routing DNS basato sulla latenza e non si basa su indirizzi IP statici.

## Destinazioni CNAME

Il servizio proxy instrada dinamicamente il traffico tra più aree utilizzando i record CNAME. Gli obiettivi attuali sono i seguenti:

| Posizione Edge | Indirizzi IP in uscita |
| --- | --- |
| Area geografica | Destinazione CNAME |
| Europa (eu-west-1) | `ethos.pub.ethos11-prod-nld2.ethos.adobe.net` |
| Stati Uniti orientali (us-east-2) | `ethos.pub.ethos11-prod-va7.ethos.adobe.net` |
| Stati Uniti orientali (us-east-1) | `ethos.pub.ethos11-prod-aus5.ethos.adobe.net` |

## Inserire nell&#39;elenco Consentiti Voci di consigliate

Inserire nell&#39;elenco Consentiti Per garantire una connettività affidabile, i seguenti nomi host:

* `ethos.pub.ethos11-prod-nld2.ethos.adobe.net`
* `ethos.pub.ethos11-prod-va7.ethos.adobe.net`
* `ethos.pub.ethos11-prod-aus5.ethos.adobe.net`

## Facoltativo: individuazione IP

Se i criteri di rete richiedono la inserisce nell&#39;elenco Consentiti di un&#39;basata su IP, è possibile visualizzare gli indirizzi IP pubblici correnti associati al servizio proxy utilizzando questo strumento:

* `DNSChecker – A Record Lookup for tnt-web-proxy.adobe.io`