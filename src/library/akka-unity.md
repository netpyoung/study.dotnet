# ref
* https://www.slideshare.net/theburningmonk/akka-for-realtime-multiplayer-mobile-games
* http://veblush.github.io/ko/posts/akkanet-online-game-works-for-the-last-2-months/

[Akka.Interfaced](https://github.com/SaladLab/Akka.Interfaced)



    MessageHandler<Tid, TReq, TRes>


udp - https://github.com/lidgren/lidgren-network-gen3


```

UDP 지원
기존 TCP 에 더해 UDP 채널을 추가했다. UDP 채널을 지원하게 된 이유는 다음과 같다.

HandOver: 모바일 환경에서 HandOver 를 TCP 로 구현하는데 추가 작업이 있다. 제대로 된 HandOver 를 구현하기 위해서는 TCP 위에다가 다시 전송 레이어를 올려야 하는데 이게 배보다 배꼽이 커지는 구현이다. 그러느니 Reliable UDP 을 가져다 쓰면 되지 않을까? 거기에 더해 TCP 는 HandOver 가 느리게 수행되는데 UDP 는 애초에 비접속 프로토콜이라 빠르게 수행될 수 있다.

다양한 QoS 지원: TCP 는 Reliable & Ordered 만 지원하는데 반해 UDP 를 가지고는 Reliable & Unordered 혹은 Unreliable 등을 추가로 지원할 수 있다. 플레이어 움직임 패킷은 unreliable-sequenced 만으로도 충분하며 TCP 에 비해 더 좋은 성능을 보인다.


```

(https://github.com/SaladLab/Akka.Interfaced.SlimSocket)
https://github.com/SaladLab/Akka.Interfaced.SlimSocket/tree/master/core/Akka.Interfaced.SlimSocket.Tests
