## HA(High Availability:고가용성)

### `HA(High Availability)` 란</br>

HA(High Availability)를 간단히 한국어로 직역을 하자면 “고가용성”이다.</br>
고 가용성이란 “가용성이 높다”는 뜻으로서, “절대 고장 나지 않음”을 의미한다.</br>
즉, 네트워크나 프로그램 등의 정보 시스템이 상당히 오랜 기간 동안 지속적으로 정상 운영이 가능한 성질을 말한다. </br></br>



### `HA(High Availability)` 특징</br>
- 2개의 서버를 이용하여 하나는 `Active` 상태, 나머지 하나는 `Standby` 상태로 정해놓는다.

- 거의 모든 부하는 Active에서 부담하고 Standby 상태의 서버는 Active 서버가 장애가 발생하지 않는 이상, 거의 가동하지 않는다.

- 실제 서비스를 운영하는 Active 서버가 어떠한 장애로 정상적인 작동이 불가능해진다면, 곧바로 Standby 서버가 Active 되면서 다시 서비스를 정상 작동할 수 있게 하는 구성이다.
