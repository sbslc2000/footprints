# Congestion
네트워크 혼잡(Congestion)이란 네트워크 내에 데이터 트래픽이 과도하게 몰려서, 라우터나 링크의 처리 용량을 초과하는 현상을 말한다. 이로 인해 패킷 지연(packet delay) 증가, 손실(packet loss) 발생, 처리율 감소 등의 문제가 발생한다.

## Congestion Scenario 1
혼잡의 원인과 비용을 알아보기 위해 몇몇 시나리오들을 분석해 볼 것이다. 첫번째 시나리오는 호스트 A와 B가 단일 라우터를 통해 데이터를 전송하며, 라우터는 무한대의 버퍼 공간을 갖는다고 가정한다.
![](https://i.imgur.com/pCMcd7o.png)
호스트 A의 애플리케이션에서 $\lambda_{in}/sec$ 의 속도로 데이터를 전송한다고 가정하자. 라우터의 출력 용량은 R로 가정한다. 호스트 B도 동일하게 동작한다.
![](https://i.imgur.com/99ooyir.png)
이 때 호스트 A에서의 처리량(좌측)과 지연(우측)은 위와 같다. 처리량은 R/2가 되는 시점까지는 전송하는 데이터가 늘어남에 따라 동일하게 늘어난다(호스트 B도 R/2만큼을 전송하기 때문). 이후에는 라우터의 출력 링크가 보내는 용량보다 입력의 용량이 많아지므로 병목이 발생하여, 처리량은 R/2로 유지된다.

지연의 경우 R/2에 가까워짐에 따라 무한대로 증가한다. 이유는 [[Queueing Delay]]에서 확인할 수 있다.

처리량이 R/2에 가까워지는 것은 링크를 최대한 활용하게 되므로 좋은 현상이다. 하지만 링크의 사용률이 높아짐에 따라 평균 지연 시간은 무제한에 가까워진다. **패킷 도착률이 링크 용량에 근접함에 따라 큐잉 지연은 커진다.**

## 혼잡 시나리오 2

## 혼잡 시나리오 3