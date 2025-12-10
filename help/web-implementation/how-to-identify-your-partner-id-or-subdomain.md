---
title: Come identificare l’ID partner o il sottodominio
description: Scopri come identificare l’ID partner o il sottodominio quando implementi alcune funzioni di Experience Cloud e in due luoghi puoi ottenere questo ID nell’interfaccia utente di Audience Manager.
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: d47848370e7bf7617f2b706041c911161a6479cd
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Come identificare il sottodominio Audience Manager {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

Quando si implementano alcune funzionalità di Experience Cloud, è necessario sapere cosa è l&#39;Audience Manager `Subdomain` (talvolta indicato anche come `client ID` o `Partner ID`). Questo video illustra due posizioni in cui puoi ottenere queste informazioni nell’interfaccia utente di Audience Manager.

## Dare via il finale... {#giving-away-the-ending}

Nel caso in cui si preferisca semplicemente saltare dentro e trovarlo senza guardare questo breve video, è possibile trovare il tuo `Partner Subdomain` in due posizioni nell&#39;interfaccia utente:

1. Se hai già creato una caratteristica [!UICONTROL rule-based], fai clic su **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] è accanto alla caratteristica nell&#39;elenco delle caratteristiche in quella cartella e l&#39;URL includerà il tuo sottodominio nell&#39;URL.
1. Se si accede all&#39;interfaccia **[!UICONTROL Tools]** > **[!UICONTROL Tags]** e si fa clic su **[!UICONTROL Get code]** per il contenitore, il sottodominio si trova verso la fine della riga Akamai

Se non riesci a trovarlo rapidamente con quei riferimenti rapidi, il video è un impegno di tempo breve. :)

>[!VIDEO](https://video.tv.adobe.com/v/327912/?captions=ita&quality=12)

>[!IMPORTANT]
>
>A ciascun cliente di Adobe Experience Cloud viene assegnato un ID numerico, spesso indicato come &quot;PID&quot; o ID partner. Questo non è l’ID di cui stiamo parlando in questo articolo e video. Il &quot;sottodominio partner&quot;, a volte indicato come ID partner, è in genere una versione del nome client ed è il sottodominio del server a cui vengono inviati i dati. Ad esempio, se la tua azienda è &quot;Bob&#39;s Knobs&quot; (tutte le cose manopole, ovviamente, haha), è probabile che il tuo sottodominio partner sia &quot;bobsknobs&quot;, mentre il &quot;PID&quot; sarebbe qualcosa di più simile a &quot;12345&quot;. In genere non è necessario conoscere il PID, ma il sottodominio è importante da conoscere, in modo da poter configurare l’implementazione di Audience Manager.
>
