---
title: "H264 Taxa de Bits Única de Baixa Qualidade SD para Android | Microsoft Docs"
description: "O tópico fornece uma visão geral da predefinição de tarefa **H264 Taxa de Bits Única de Baixa Qualidade Para Android**."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 67d3446d-e3b8-419f-bbf8-e5149c108531
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: 55bfd5b283e6159e0a06b3107de31d1722796bf1
ms.openlocfilehash: 37f3a2b50b437fd675c17a26a143eb649fb6c857
ms.contentlocale: pt-br
ms.lasthandoff: 11/29/2016

---

# <a name="h264-single-bitrate-low-quality-sd-for-android"></a>H264 Taxa de Bits Única de Baixa Qualidade SD para Android
`Media Encoder Standard` define um conjunto de predefinições de codificação que pode ser usado ao criar trabalhos de codificação. Você pode usar um `preset name` para especificar em qual formato deseja codificar o arquivo de mídia. Ou, pode criar suas próprias predefinições com base em JSON ou XML (usando a codificação UTF-8 ou UTF-16). Em seguida, você passaria a predefinição personalizada ao codificador. Para obter a lista de todos os nomes de predefinição com suporte por este codificador `Media Encoder Standard`, consulte [Predefinições de tarefa para o Media Encoder Standard](media-services-mes-presets-overview.md).  
  
 Este tópico mostra a predefinição `H264 Single Bitrate Low Quality SD for Android` no formato XML e JSON.  
  
 Essa predefinição produz um único arquivo MP4 com uma taxa de bits de 56 kbps e áudio AAC estéreo. Para obter informações detalhadas sobre o perfil, taxa de bits, taxa de amostragem etc. dessa predefinição, examine o XML ou JSON definido abaixo. Para obter explicações sobre o significado de cada elemento nessas predefinições, e os valores válidos para cada elemento, veja o tópico [Esquema do Media Encoder Standard](media-services-mes-schema.md).  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:05</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>56</Bitrate>  
          <Width>176</Width>  
          <Height>144</Height>  
          <FrameRate>12/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>2</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>56</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>HEAACV2</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>24</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:05",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Baseline",  
          "Level": "2",  
          "Bitrate": 56,  
          "MaxBitrate": 56,  
          "BufferWindow": "00:00:05",  
          "Width": 176,  
          "Height": 144,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
          "Type": "H264Layer",  
          "FrameRate": "12/1"  
        }  
      ],  
      "Type": "H264Video"  
    },  
    {  
      "Profile": "HEAACV2",  
      "Channels": 2,  
      "SamplingRate": 48000,  
      "Bitrate": 24,  
      "Type": "AACAudio"  
    }  
  ],  
  "Outputs": [  
    {  
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
      "Format": {  
        "Type": "MP4Format"  
      }  
    }  
  ]  
}  
```

