---
keywords: implementa, implementa, whitelist, whitelist, inserisce nell'elenco Consentiti di, elenco consentiti, edge, edge, 9 $
description: Visualizza un elenco di host per aiutarti a inserire nell'elenco Consentiti [!DNL Adobe Target] gli spigoli (nodi di servizio geograficamente distribuiti che garantiscono tempi di risposta ottimali agli utenti finali).
title: Inserire nell'elenco Consentiti Come posso  [!DNL Target] nodi Edge?
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 275c3fabdcaf3152d7d6161f3c325e54c840c805
workflow-type: tm+mt
source-wordcount: '563'
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
>
>Tutti gli indirizzi Edge4 *x* elencati in entrambe le tabelle seguenti sono pianificati per l’aggiornamento il 9 agosto 2023.

## Indirizzi IP NAT (Network Address Translation) di [!DNL Target] reti Edge

Elenco di indirizzi IP in uscita di [!DNL Target] Edge. Inserire nell&#39;elenco Consentiti questi IP se si prevede di avere [!DNL Target] accesso ai propri servizi.

| Posizione Edge | Indirizzi IP in uscita |
| --- | --- |
| Edge41 (Mumbai) | 3.6.2.221<P>13.235.112.4 <P>52.66.66.192 |
| Edge42 (Tokyo) | 52.69.55.232<P>43.206.61.43 <P>13.113.73.214 |
| Edge44 (costa orientale USA) | 54.164.192.223<P>52.86.86.203 <P>54.88.167.98 |
| Edge45 (costa occidentale degli Stati Uniti) | 52.40.124.129<P>54.148.219.69 <P>54.189.208.212 |
| Edge46 (Sydney) | 54.253.144.4<P>54.66.198.142 <P>13.211.218.51 |
| Edge47 (Irlanda) | 52.208.136.136<P>54.170.28.19 <P>99.80.111.82 |
| Edge48 (Singapore) | 3.1.141.36<P>18.143.112.116 <P>52.76.61.44 |

## [!DNL Target] indirizzi IP edge

Elenco di indirizzi IP di [!DNL Target] Edge. Se desideri effettuare chiamate API a [!DNL Target] Edge, Inserisce nell&#39;elenco Consentiti questi IP.

Questo elenco verrà modificato spesso, man mano che i load balancer aumentano o diminuiscono in base ai profili di traffico.

| Posizione Edge | Dominio | Indirizzi IP |
| --- | --- | --- |
|  | `CLIENTCODE.tt.omtrdc.net`<br />(dove CLIENTCODE è l&#39;ID client [!DNL Target]) |  |
| Edge41 (Mumbai) | `mboxedge41.tt.omtrdc.net` | 3.6.2.221<P>52.66.66.192<P>13.235.112.4 |
| Edge42 (Tokyo) | `mboxedge42.tt.omtrdc.net` | 43.206.61.43<P>13.113.73.214<P>52.69.55.232 |
| Edge44 (costa orientale USA) | `mboxedge44.tt.omtrdc.net` | 54.88.167.98<P>54.164.192.223<P>52.86.86.203 |
| Edge45 (costa occidentale degli Stati Uniti) | `mboxedge45.tt.omtrdc.net` | 52.40.124.129<P>54.148.219.69<P>54.189.208.212 |
| Edge46 (Sydney) | `mboxedge46.tt.omtrdc.net` | 54.66.198.142<P>54.253.144.4<P>13.211.218.51 |
| Edge47 (Irlanda) | `mboxedge47.tt.omtrdc.net` | 54.170.28.19<P>52.208.136.136<P>99.80.111.82 |
| Edge48 (Singapore) | `mboxedge48.tt.omtrdc.net` | 52.76.61.44<P>3.1.141.36<P>18.143.112.116 |

Man mano che i load balancer rilevano le modifiche nel profilo di traffico, questo si espande verso l’alto o verso il basso. Il tempo necessario per la scalabilità di Elastic Load Balancing può variare da 1 a 7 minuti, a seconda delle modifiche rilevate. Quando i load balancer vengono scalati, aggiornano il record DNS con il nuovo elenco di indirizzi IP. Per garantire che si stia sfruttando la maggiore capacità, il bilanciamento del carico elastico utilizza un’impostazione TTL sul record DNS di 60 secondi.

## requisiti di Inserisce nell&#39;elenco Consentiti per il servizio proxy [!DNL Target]

Per garantire un accesso ininterrotto a [!DNL Target] tramite [!DNL Experience Edge Connector] (EEC), i clienti potrebbero dover aggiornare le configurazioni di rete per consentire il traffico verso il servizio proxy.

### Panoramica del servizio proxy

* **Endpoint servizio**: `https://tnt-web-proxy.adobe.io`.
* **Infrastruttura**: ospitata sulla piattaforma Ethos [!DNL Adobe].
* **Nota**: questo servizio utilizza il routing DNS basato sulla latenza e non si basa su indirizzi IP statici.

### Destinazioni CNAME

Il servizio proxy instrada dinamicamente il traffico tra più aree utilizzando i record CNAME. Gli obiettivi attuali sono i seguenti:

| Posizione Edge | Indirizzi IP in uscita |
| --- | --- |
| Area geografica | Destinazione CNAME |
| Europa (eu-west-1) | `ethos.pub.ethos11-prod-nld2.ethos.adobe.net` |
| Stati Uniti orientali (us-east-2) | `ethos.pub.ethos11-prod-va7.ethos.adobe.net` |
| Stati Uniti orientali (us-east-1) | `ethos.pub.ethos11-prod-aus5.ethos.adobe.net` |

### Inserire nell&#39;elenco Consentiti Voci di consigliate

Inserire nell&#39;elenco Consentiti Per garantire una connettività affidabile, i seguenti nomi host:

* `ethos.pub.ethos11-prod-nld2.ethos.adobe.net`
* `ethos.pub.ethos11-prod-va7.ethos.adobe.net`
* `ethos.pub.ethos11-prod-aus5.ethos.adobe.net`

### Facoltativo: individuazione IP

Se i criteri di rete richiedono la inserisce nell&#39;elenco Consentiti di un&#39;basata su IP, è possibile visualizzare gli indirizzi IP pubblici correnti associati al servizio proxy utilizzando questo strumento:

* `DNSChecker – A Record Lookup for tnt-web-proxy.adobe.io`