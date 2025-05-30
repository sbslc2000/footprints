---
상위 개념: "[[Packet Switching]]"
---
# Queueing Delay
![](https://i.imgur.com/Fw90gyq.png)

만약 한 패킷이 라우터에 도착했을 때, 출력 포트를 이미 다른 패킷이 사용중이라면 패킷은 전송되기 위하여 큐에서 대기해야 한다. 이렇게 기다리는 현상 혹은 기다리는 기간을 '큐잉 지연'이라고 한다.

## Traffic Intensity
큐잉 지연은 언제 크고, 언제 미미한가? 이는 트래픽 강도(Traffic Intensity)와 링크의 전송률에 달려있다.

트래픽 강도란 비트가 큐에 도착하는 평균율이다. 단순함을 위해 패킷의 비트 수가 L이고 패킷이 초 당 도착하는 평균 횟수를 a라고 할 때, 트래픽 강도는 La bit/sec 로 구할 수 있다. 링크의 전송률은 출력 포트로 비트를 밀어낼 수 있는 비율로 R bps를 갖는다고 하자. 

La / R 이라는 값은 큐잉 지연의 정도를 측정하는데에 매우 중요하다.

La / R > 1 이라면, 비트를 내보내는 수 보다 들어오는 비트 수가 더 많으므로 큐의 데이터는 무한대로 쌓일 것이며 큐잉 지연 역시 무한대로 증가한다. 물론 현실에서는 큐는 제한된 크기를 가지므로 대부분의 패킷이 손실되는 결과를 낳을 것이다.

La / R <= 1인 경우는, 도착 트래픽의 특성이 큐잉 지연에 영향을 미친다. 예를 들어 하나의 패킷이 L/R 초마다 도착한다면, 패킷은 항상 바로 전송될 것이고 큐잉 지연은 없다. 하지만 패킷이 몰려서 도착한다면, 상당한 큐잉 지연이 생길 것이다.

![](https://i.imgur.com/4eM0EGp.png)

큐잉 이론에 따르면 트래픽 강도는 1에 접근할수록 평균 큐잉 지연은 급속이 증가하게 된다. 초당 처리할 수 있는 비트 수와 트래픽의 비트 수가 동일할 때, 큐잉 지연이 무한대가 된다는 것이 아이러니하지만, 이는 패킷이 일정하지 않고 **무작위로 도착한다는 네트워크의 본질적인 특성** 때문이다. 일정한 간격으로 패킷이 도착하면 충분히 처리 가능하지만, 현실에서는 트래픽이 순간적으로 몰리는 경우가 많기 때문에, 처리율과 도착율이 같더라도 **순간적인 혼잡이 큐의 적체를 유발하고**, 이는 **평균 큐잉 지연의 급증**으로 이어지게 된다.