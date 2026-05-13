---
title: Migrazione dal server di tracciamento all’inoltro lato server a livello di suite di rapporti
description: Scopri come abilitare l’inoltro lato server dei dati di Adobe Analytics ad Audience Manager a livello di suite di rapporti invece che a livello di server di tracciamento.
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
TQID: https://experienceleague.adobe.com/-fWEu9LWHY-PtIZ-7Phf-ZOHPCD-A67mwb9i3kA7nec
product_v2:
  - id: df80eeb1-8d72-467e-b0df-9d51c7d3a0a1
feature_v2:
  - id: a8b0238e-1d43-4679-a3b4-5ba1bad83baa
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
source-git-commit: 3152e8fc51e0e06c90c17dce0aa203a27995e88d
workflow-type: tm+mt
source-wordcount: 608
ht-degree: 0%

---

# Migrazione dal server di tracciamento all’inoltro lato server a livello di suite di rapporti {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

In questo articolo e in questo video verrà illustrato come abilitare l&#39;inoltro lato server dei dati [!DNL Analytics] ad Audience Manager a livello [!UICONTROL report suite] anziché a livello [!UICONTROL tracking server].

## Introduzione {#introduction}

Se si dispone di Adobe Audience Manager E Adobe Analytics, è possibile implementare l&#39;inoltro lato server dei dati [!DNL Analytics] ad Audience Manager. Questo significa che, invece di inviare due hit (uno a [!DNL Analytics] e uno ad Audience Manager), la pagina può inviare un hit a [!DNL Analytics] e [!DNL Analytics] inoltrerà tali dati ad Audience Manager.

Se disponi già di questo elemento in esecuzione e se lo avevi abilitato/implementato prima di ottobre 2017, l&#39;inoltro lato server potrebbe essere basato su [!UICONTROL Tracking Server], che doveva essere abilitato dall&#39;Assistenza clienti di Adobe o da Adobe Consulting. A partire da ottobre 2017, è ora possibile configurare autonomamente l’inoltro lato server e a livello di suite di rapporti (inoltro per suite di rapporti). Questo comporta vantaggi significativi, che vengono discussi di seguito.

## Inoltro di [!UICONTROL Tracking server] {#tracking-server-forwarding}

[!UICONTROL tracking server] è la posizione in cui si inviano i dati di [!DNL Analytics] e il dominio in cui vengono scritti la richiesta di immagine e il cookie. Deve essere impostato in DTM o [!DNL Experience Platform Launch] oppure nel file [!DNL AppMeasurement.js] e avrà in genere questo aspetto, con il nome del sito o dell&#39;azienda che sostituisce &quot;miosito&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

Se l&#39;inoltro lato server è configurato per l&#39;inoltro al livello [!UICONTROL tracking server], qualsiasi hit inviato a questo [!UICONTROL tracking server] (SE è abilitato anche il servizio Experience Cloud ID) verrà inoltrato ad Audience Manager. Questa funzione doveva essere abilitata dall’Assistenza clienti di Adobe o da Adobe Consulting. Sono anche gli utenti che possono disabilitarlo, DOPO il passaggio all&#39;inoltro [!UICONTROL report suite], come descritto di seguito.

Se non sei sicuro se [!DNL tracking server forwarding] è abilitato, contatta l&#39;Assistenza clienti Adobe o Adobe Consulting, che dovrebbero essere in grado di comunicarti.

## Inoltro lato server a livello di [!UICONTROL Report-suite] {#report-suite-level-server-side-forwarding}

Uno dei principali vantaggi del passaggio all&#39;inoltro [!UICONTROL report suite] dall&#39;inoltro [!UICONTROL tracking server] è la possibilità di utilizzare &quot;Audience Analytics&quot;, ovvero la possibilità di inoltrare nuovamente Audience Manager [!UICONTROL segments] ad Adobe Analytics per l&#39;analisi dettagliata dei segmenti. Questa funzionalità avanzata NON è supportata se si è ancora in fase di inoltro [!UICONTROL tracking server] e non di inoltro [!UICONTROL report suite]. Ulteriori informazioni su Audience Analytics sono disponibili nella [documentazione](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## Suggerimento importante {#additional-resources}

Come indicato nel video precedente, una volta che hai impostato tutti i [!UICONTROL report suites] da inoltrare ad Audience Manager, devi contattare l&#39;Assistenza clienti di Adobe o Adobe Consulting e far disabilitare l&#39;inoltro di [!UICONTROL tracking server]. Questa operazione non rappresenta un&#39;emergenza, perché l&#39;inoltro di [!UICONTROL tracking server] e l&#39;inoltro di [!UICONTROL report suite] non generano hit duplicati. Tuttavia, è consigliabile avere solo l&#39;inoltro [!UICONTROL report suite] su.

Se si lascia in [!UICONTROL tracking server] l&#39;inoltro, non solo potrebbe inoltrare i dati da [!UICONTROL report suites] che non si desidera inoltrare, ma in futuro, dopo che l&#39;utente (e tutti coloro che fanno parte della società) hanno dimenticato che l&#39;inoltro di [!UICONTROL tracking server] è attivo, si potrebbe pensare che i dati non vengano inoltrati per un [!UICONTROL report suite] specifico. Questo perché non è attivato a livello di suite di rapporti, ma i dati vengono comunque inoltrati a causa di [!UICONTROL tracking server]. Poi sprecherai tempo e denaro per capire perché inoltra e paga per chiamate server AAM che non ti aspettavi. È quindi consigliabile disattivare l&#39;inoltro di [!UICONTROL tracking server] non appena tutti i [!UICONTROL report suites] sono impostati per l&#39;inoltro, in base alle esigenze aziendali.
