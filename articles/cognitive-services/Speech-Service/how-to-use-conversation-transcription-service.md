---
title: Speech SDK-음성 서비스를 사용 하 여 다중 참가자 대화 기록
titleSuffix: Azure Cognitive Services
description: Speech SDK를 사용 하 여 대화 기록을 사용 하는 방법에 알아봅니다. 에 사용할 수 있는 C++, C#, 및 Java 합니다.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jhakulin
ms.openlocfilehash: 80ec606fee30c239d47bca94188d3b9cbb7c82d5
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604420"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>Speech SDK를 사용 하 여 다중 참가자 대화 기록

Speech SDK **ConversationTranscriber** API를 사용 하면 추가, 제거 및 스트리밍 오디오 음성 서비스 사용 하 여 참가자를 식별 하는 기능을 사용 하 여 회의/대화를 촬영할 수 `PullStream` 또는`PushStream`.

## <a name="limitations"></a>제한 사항

* 대화 필기 인식기 대해서는 C++, C#, 및 Windows, Linux 및 Android에서 Java 합니다.
* ROOBO DevKit에 화자 식별을 효율적으로 활용할 수 있는 순환 다중 마이크 배열을 제공 하는 대화 기록 만들기에 대 한 지원 되는 하드웨어 환경입니다. [자세한 내용은 음성 장치 SDK를 참조 하세요.](speech-devices-sdk.md)합니다.
* Speech SDK 지원은 대화 기록에 대 한 오디오 끌어오기를 사용 하 여 8 개의 16 비트 16 44.1khz PCM 오디오 채널을 사용 하 여 모드 스트림을 푸시 제한 됩니다.
* 대화 기록 현재 다음 지역에서 "EN-US" 및 "ZH-CN" 언어로 제공 됩니다: centralus 및 eastasia 합니다.

## <a name="prerequisites"></a>필수 조건

* [Speech SDK를 사용 하 여 음성-텍스트를 사용 하는 방법에 알아봅니다.](quickstart-csharp-dotnet-windows.md)
* [음성 평가판 구독을 가져옵니다.](https://azure.microsoft.com/try/cognitive-services/)
* Speech SDK 버전 1.5.1 이상 버전이 필요 합니다.

## <a name="create-voice-signatures-for-participants"></a>참가자에 대 한 음성 시그니처를 만듭니다

첫 번째 단계 대화 참가자에 대 한 음성 시그니처를 만드는 것입니다. 음성 서명을 만드는 것이 효율적인 화자 식별 필요 합니다.

### <a name="requirements-for-input-wave-file"></a>입력된 wave 파일에 대 한 요구 사항

* 음성 시그니처를 만들기 위한 입력된 오디오 wave 파일에서 16 비트 샘플, 16 kHz 샘플 속도 및 단일 채널 (Mono) 형식 이어야 합니다.
* 각 오디오 샘플에 대 한 권장 되는 길이 30 초에서 2 분 사이입니다.

다음 예제에서는 두 가지 방법으로 [를 사용 하 여 REST API] 음성 시그니처를 만들려면 (https://aka.ms/cts/signaturegenservice) 에서 C#:

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribing-conversations"></a>대화 복사

여러 참가자와 대화를 촬영할 수 만들기는 `ConversationTranscriber` 연관 된 개체를 `AudioConfig` conversation 세션 및 스트림에 오디오를 사용 하 여 만든 개체 `PullAudioInputStream` 또는 `PushAudioInputStream`.

호출 ConversationTranscriber 클래스가 있다고 가정해 보겠습니다 `MyConversationTranscriber`합니다. 코드는이 처럼 보일 수 있습니다.

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Conversation;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and region.
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file.
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            // Creates a conversation transcriber using audio stream input.
            using (var transcriber = new ConversationTranscriber(config, audioInput))
            {
                // Subscribes to events.
                transcriber.Recognizing += (s, e) =>
                {
                    Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                };

                transcriber.Recognized += (s, e) =>
                {
                    if (e.Result.Reason == ResultReason.RecognizedSpeech)
                    {
                        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                    }
                    else if (e.Result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                };

                transcriber.Canceled += (s, e) =>
                {
                    Console.WriteLine($"CANCELED: Reason={e.Reason}");

                    if (e.Reason == CancellationReason.Error)
                    {
                        Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                        Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                        Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        stopTranscription.TrySetResult(0);
                    }
                };

                transcriber.SessionStarted += (s, e) =>
                {
                    Console.WriteLine("\nSession started event.");
                };

                transcriber.SessionStopped += (s, e) =>
                {
                    Console.WriteLine("\nSession stopped event.");
                    Console.WriteLine("\nStop recognition.");
                    stopTranscription.TrySetResult(0);
                };

                // Sets a conversation Id.
                transcriber.ConversationId = "AConversationFromTeams";

                // Add participants to the conversation.
                // Create voice signatures using REST API described in the earlier section in this document. 
                // Voice signature needs to be in the following format:
                // { "Version": <Numeric value>, "Tag": "string", "Data": "string" }

                var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                transcriber.AddParticipant(speakerA);
                transcriber.AddParticipant(speakerB);
                transcriber.AddParticipant(speakerC);

                // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                await transcriber.StartTranscribingAsync().ConfigureAwait(false);

                // Waits for completion.
                // Use Task.WaitAny to keep the task rooted.
                Task.WaitAny(new[] { stopTranscription.Task });

                // Stop transcribing the conversation.
                await transcriber.StopTranscribingAsync().ConfigureAwait(false);

                // Ends the conversation.
                await transcriber.EndConversationAsync().ConfigureAwait(false);
            }
        }
    }
}
```

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [GitHub에서 샘플 살펴보기](https://aka.ms/csspeech/samples)
