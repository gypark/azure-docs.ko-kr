---
title: Azure Application Insights를 사용하여 OpenCensus Python 추적 | Microsoft Docs
description: OpenCensus Python 추적을 로컬 전달자 및 Application Insights와 연결하는 지침을 제공합니다.
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ae9db483e15197e6cdaaaa5981410630184cc6ca
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957232"
---
# <a name="collect-distributed-traces-from-python-preview"></a>Python(미리 보기)에서 분산 추적 수집

이제 Application Insights는 [OpenCensus](https://opencensus.io) 및 새로운 [로컬 전달자](./../../azure-monitor/app/opencensus-local-forwarder.md)와의 통합을 통해 Python 애플리케이션의 분산 추적을 지원합니다. 이 문서에서는 Python에 대해 OpenCensus를 설정하고 추적 데이터를 Application Insights로 가져오는 프로세스를 단계별로 안내합니다.

## <a name="prerequisites"></a>필수 조건

- Azure 구독이 필요합니다.
- Python을 설치해야 하며, 이전 버전은 사소한 조정이 작동할 가능성이 있지만 이 문서에서는 [Python 3.7.0](https://www.python.org/downloads/)을 사용합니다.
- 지침에 따라 [로컬 전달자를 Windows 서비스로](./../../azure-monitor/app/opencensus-local-forwarder.md) 설치합니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험](https://azure.microsoft.com/free/) 계정을 만듭니다.

## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인

[Azure Portal](https://portal.azure.com/)에 로그인합니다.

## <a name="create-application-insights-resource"></a>Application Insights 리소스 만들기

먼저 계측 키(ikey)를 생성할 Application Insights 리소스를 만들어야 합니다. 그런 다음, ikey를 사용하여 OpenCensus 계측 애플리케이션에서 Application Insights로 분산 추적을 보내도록 로컬 전달자를 구성합니다.   

1. **리소스 만들기** > **개발자 도구** > **Application Insights**를 선택합니다.

   ![Application Insights 리소스 추가](./media/opencensus-python/0001-create-resource.png)

   구성 상자가 표시되면 다음 표를 사용하여 입력 필드를 채웁니다.

    | 설정        | 값           | 설명  |
   | ------------- |:-------------|:-----|
   | **Name**      | 전역적으로 고유한 값 | 모니터링하는 응용 프로그램을 식별하는 이름입니다. |
   | **애플리케이션 유형** | 일반 | 모니터링하는 응용 프로그램의 유형입니다. |
   | **리소스 그룹**     | myResourceGroup      | Application Insights 데이터를 호스팅할 새 리소스 그룹의 이름입니다. |
   | **위치**: | 미국 동부 | 가까운 위치 또는 응용 프로그램이 호스팅되는 위치 근처를 선택합니다. |

2. **만들기**를 클릭합니다.

## <a name="configure-local-forwarder"></a>로컬 전달자 구성

1. **개요** > **기본 정보**를 차례로 선택하고, 애플리케이션의 **계측 키**를 복사합니다.

   ![계측 키 스크린샷](./media/opencensus-python/0003-instrumentation-key.png)

2. `LocalForwarder.config` 파일을 편집하고 계측 키를 추가합니다. [필수 조건](./../../azure-monitor/app/opencensus-local-forwarder.md)의 지침을 따른 경우 이 파일은 `C:\LF-WindowsServiceHost`에 있습니다.

    ```xml
      <OpenCensusToApplicationInsights>
        <!--
          Instrumentation key to track telemetry to.
          -->
        <InstrumentationKey>{enter-instrumentation-key}</InstrumentationKey>
      </OpenCensusToApplicationInsights>
    
      <!-- Describes aspects of processing Application Insights telemetry-->
      <ApplicationInsights>
        <LiveMetricsStreamInstrumentationKey>{enter-instrumentation-key}</LiveMetricsStreamInstrumentationKey>
      </ApplicationInsights>
    </LocalForwarderConfiguration>
    ```

3. 애플리케이션 **로컬 전달자** 서비스를 다시 시작합니다.

## <a name="opencensus-python-package"></a>OpenCensus Python 패키지

1. Python 및 pip 또는 명령줄에서 pipenv를 사용 하 여 내보내기에 대 한 열기 인구 조사 패키지를 설치 합니다.

    ```console
    python -m pip install opencensus
    python -m pip install opencensus-ext-ocagent

    # pip env install opencensus
    ```

    > [!NOTE]
    > `python -m pip install opencensus`는 Python 설치에 대해 PATH 환경 변수를 설정했다고 가정합니다. 아직 구성하지 않은 경우 `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus`와 같은 명령이 되는 Python 실행 파일이 위치하는 곳에 전체 디렉터리 경로를 제공해야 합니다.

2. 먼저 일부 추적 데이터를 로컬로 생성해 보겠습니다. Python IDLE에서 또는 원하는 편집기에서 다음 코드를 입력합니다.

    ```python
    from opencensus.trace.tracer import Tracer

    def main():
        while True:
            valuePrompt()

    def valuePrompt():
        tracer = Tracer()
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    if __name__ == "__main__":
        main()

    ```

3. 코드를 실행하면 값을 입력하라는 메시지가 반복적으로 표시됩니다. 각 항목을 사용하여 값은 셸에 출력되고, **SpanData**의 해당 부분이 OpenCensus Python 모듈에 의해 생성됩니다. OpenCensus 프로젝트는 [_범위 트리로 추적_](https://opencensus.io/core-concepts/tracing/)을 정의합니다.
    
    ```python
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=1f07f062ac394c50925f2ae61e635e14, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='5c17a4ad6ba14299', parent_span_id=None, attributes={}, start_time='2018-09-15T20:42:15.847292Z', end_time='2018-09-15T20:42:17.615664Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=c71b4e88a22a495da61df52ce3eee3e1, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='51547c0af5f046eb', parent_span_id=None, attributes={}, start_time='2018-09-15T20:42:17.615664Z', end_time='2018-09-15T20:48:11.160314Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=b4cdcc9e6df44a8fbb6e8ddeccc1351c, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f2caacf7892744d1', parent_span_id=None, attributes={}, start_time='2018-09-15T20:48:11.175931Z', end_time='2018-09-15T20:48:12.629178Z', child_span_count=0, stack_trace=None, time_events=[], links=[], status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

4. 데모용으로 유용하지만, 궁극적으로 **로컬 전달자 서비스**로 선택되고 Application Insights에 전송될 수 있는 방법으로 SpanData를 내보내려고 합니다. 이전 단계의 코드를 다음으로 수정합니다.

    ```python
    from opencensus.trace.tracer import Tracer
    from opencensus.trace import config_integration
    from opencensus.ext.ocagent.trace_exporter import TraceExporter
    from opencensus.trace import tracer as tracer_module

    import os

    def main():
        while True:
            valuePrompt()

    def valuePrompt():
        export_LocalForwarder = TraceExporter(
        service_name=os.getenv('SERVICE_NAME', 'python-service'),
        endpoint=os.getenv('OCAGENT_TRACE_EXPORTER_ENDPOINT'))

        tracer = Tracer(exporter=export_LocalForwarder)
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    if __name__ == "__main__":
        main()

    ```

5. 위의 모듈을 저장하고 실행하는 경우 `grpc`에 대한 `ModuleNotFoundError`를 받을 수 있습니다. 이 오류가 발생하는 경우 다음을 실행하여 다음과 함께 [grpcio 패키지](https://pypi.org/project/grpcio/)를 설치합니다.

    ```console
    python -m pip install grpcio
    ```

6. 이제 위에서 Python 스크립트를 실행할 때 값을 입력하라는 메시지가 여전히 표시되지만 이제는 값만 셸에 인쇄됩니다.

7. **로컬 전달자**가 추적을 선택하는지 확인하려면 `LocalForwarder.config` 파일을 확인합니다. [필수 구성 요소](https://docs.microsoft.com/azure/application-insights/local-forwarder)의 단계를 따른 경우 `C:\LF-WindowsServiceHost`에 배치됩니다.

    로그 파일의 아래 이미지에서 내보내기를 추가한 두 번째 스크립트를 실행하기 전에 `OpenCensus input BatchesReceived`가 0인 것을 확인할 수 있습니다. 업데이트된 스크립트 실행을 시작했을 때 `BatchesReceived`가 입력한 값의 수와 동일하게 증가했습니다.
    
    ![새로운 App Insights 리소스 형식](./media/opencensus-python/0004-batches-received.png)

## <a name="start-monitoring-in-the-azure-portal"></a>Azure Portal에서 모니터링 시작

1. 이제 Azure Portal에서 Application Insights **개요** 페이지를 다시 열어 현재 실행 중인 애플리케이션에 대한 세부 정보를 볼 수 있습니다. **라이브 메트릭 스트림**을 선택합니다.

   ![빨간색 상자에서 라이브 메트릭 스트림이 선택된 개요 창의 스크린샷](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. 두 번째 Python 스크립트를 다시 실행하고 값 입력을 시작하는 경우 로컬 전달자 서비스에서 Application Insights에 도착하는 대로 라이브 추적 데이터가 표시됩니다.

   ![성능 데이터가 표시된 라이브 메트릭 스트림의 스크린샷](./media/opencensus-python/0006-stream.png)

3. **개요** 페이지로 다시 이동하고 종속성 관계의 시각적 개체 레이아웃에 대한 **애플리케이션 맵**을 선택하고 애플리케이션 구성 요소 간의 타이밍을 호출합니다.

    ![기본 애플리케이션 맵의 스크린샷](./media/opencensus-python/0007-application-map.png)

    하나의 메서드 호출만 추적했으므로 애플리케이션 맵은 흥미롭지 않습니다. 하지만 애플리케이션은 훨씬 분산된 애플리케이션을 시각화하도록 확장될 수 있습니다.

   ![애플리케이션 맵](media/opencensus-python/application-map.png)

4. **성능 조사**를 선택하여 자세한 성능 분석을 수행하고 성능 저하의 근본 원인을 확인합니다.

    ![성능 창의 스크린샷](./media/opencensus-python/0008-performance.png)

5. **샘플**을 선택한 다음, 오른쪽 창에 표시되는 샘플 중 하나를 클릭하면 종단 간 트랜잭션 세부 정보 환경이 시작됩니다. 샘플 앱은 단일 이벤트만을 표시하지만 더 복잡한 애플리케이션을 통해 개별 이벤트의 호출 스택의 수준까지 엔드투엔드 트랜잭션을 탐색할 수 있습니다.

     ![엔드투엔드 트랜잭션 인터페이스의 스크린샷](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="opencensus-trace-for-python"></a>Python용 OpenCensus 추적

Python용 OpenCensus를 로컬 전달자 및 Application Insights에 연결하는 기본 사항만을 다루었습니다. 공식 사용 지침에서는 다음과 같은 좀 더 고급 항목을 다룹니다.

* [샘플러](https://opencensus.io/api/python/trace/usage.html#samplers)
* [Flask 통합](https://opencensus.io/api/python/trace/usage.html#flask)
* [Django 통합](https://opencensus.io/api/python/trace/usage.html#django)
* [MySQL 통합](https://opencensus.io/api/python/trace/usage.html#service-integration)
* [PostgreSQL](https://opencensus.io/api/python/trace/usage.html#postgresql)
  
## <a name="next-steps"></a>다음 단계

* [OpenCensus Python 사용 가이드](https://opencensus.io/api/python/trace/usage.html)
* [애플리케이션 맵](./../../azure-monitor/app/app-map.md)
* [종단 간 성능 모니터링](./../../azure-monitor/learn/tutorial-performance.md)
