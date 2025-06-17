---
상위 개념: "[[TCP Reliable Transmission]]"
---
# TimeoutInterval
TCP는 신뢰적인 전송을 위한 Timeout 기간을 RTT의 예측치에 약간의 여유 기간을 두어 설정한다. 이 값을 TimeoutInterval이라는 변수로 다룬다.

$$TimeoutInterval = EstimatedRTT + 4 \times DevRTT$$

하지만 이 값은 SampleRTT를 토대로 도출된다. 따라서 최초의 요청에 대해서는 효율적인 TimeoutInterval를 알 수 없다. \[RFC 6298]에서는 TimeoutInterval의 초기 값으로 1초를 권고하고 있다.

## SampleRTT
SampleRTT는 실제로 세그먼트가 송신된 시간과 ACK이 도착한 시간까지의 길이이다. TCP는 매 세그먼트에 대해 SampleRTT를 측정하지 않고, 한 순간에 하나의 세그먼트에 대해서만 SampleRTT를 측정한다.(이는 대략 RTT마다 SampleRTT의 새로운 값을 얻게되는 효과를 낳는다)

재전송되는 세그먼트에 대해서는 SampleRTT를 계산하지 않는다.

> [!info] TCP는 왜 재전송되는 세그먼트에 대해 SampleRTT를 계산하지 않을까?
> 세그먼트가 재전송되었다는 것은 
>  - 원래 보낸 패킷이 손실되었거나
>  - ACK이 지연되었거나
>  - 네트워크 혼잡이 발생한 상황이다.
>
> 이 때 받은 ACK은 원래 전송분에 대한 ACK일 수도 있고, 재전송분에 대한 ACK일 수도 있다. 만약 원래 전송분에 대한 ACK을 받는 경우 RTT를 과소평가하게 된다. 이를 Ambiguous ACK Problem이라고도 부른다.

## EstimatedRTT
SampleRTT의 값은 라우터의 혼잡과 종단 시스템에서의 부하 변화 때문에 불규칙하다. 따라서 여러 SampleRTT의 값들을 토대로 평균값을 도출하여 RTT를 예측한다.

RTT 예측값인 EstimatedRTT는 다음과 같은 방식으로 계산된다.


$$EstimatedRTT = (1 - \alpha) \times EsteimatedRTT + \alpha \times SampleRTT $$

이는 a값 만큼 최근의 SampleRTT에 더 높은 가중치를 두어 RTT를 예측하게 된다. 최신 샘플들이 현재 네트워크의 혼잡도를 더 잘 반영하기 때문이다.

![](https://i.imgur.com/j2gdNpr.png)

이렇게 구해진 EstimatedRTT는 변화율이 높은 SampleRTT를 기반으로 예측치를 완만히 이동하게 만들어주며, 이 과정에서 과거의 데이터들은 지수적으로 영향력이 감소된다. 이러한 방식을 지수적 가중 이동 평균(Exponential Weighted Moving Average, EWMA)라고도 부른다.

권장되는 가중치 값은 0.125이다. \[RFC 6298]

## DevRTT
RTT의 변화율을 측정하는 것도 매우 중요하다. DevRTT는 SampleRTT가 EstimatedRTT로부터 얼마나 많이 벗어나는지를 예측하는 지표다.
$$DevRTT = (1 - \beta) \times DevRTT + B \times |SampleRTT - EstimatedRTT | $$
$\beta$의 권장값은 0.25이다.