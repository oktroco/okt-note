# E2E 테스트 (End to End)

사용자의 관점의 테스트. 사용자가 직접 사용하는 것과 같은 환경으로 테스트하는 것을 이야기한다.<br>
그렇기 때문에 실제로 사용자가 마주하는 UI단에서 테스트를 하는 것이 일반적이다.<br>
사용자가 사용을 하는 시나리오를 짜서 그것을 UI단에서 테스트를 하여 정말 사용자가 이용하는 것처럼 테스트한다.<br>
유닛 테스트 등으로 캐치할 수 없는 사용자의 저니맵을 테스트할 수 있다.<br>

## 테스트 시스템에 대해 고려하는 포인트
### 초창기 스타트업에 테스트 시스템이 필수적일까?
비즈니스 로직이 계속 바뀌는 초창기 스타트업 입장에서는 테스트에 익숙하지 않은 개발팀일 경우, 당장 테스트 시스템을 도입하기가 매우 어렵다.<br>
당장 시간들여서 만든 api 테스트들이 하루아침에 요구사항이 바뀌고 사라질 수도 있기 때문이다.<br>
그리고 새로운 api도 계속 생겨나는데, 이 때 테스트코드를 작성하는 시간이 +@로 들어간다면 PMF를 맞춰나가는 입장에서는 매우 시간낭비가 심하다.<br>
물론 테스트 코드의 장점으로 많이 꼽는 이슈가 "지금 1시간이 10시간을 아낄 수 있다" 라는 논리인데,<br>
PMF를 당장 맞춰가야하는 초기 스타트업 입장에서는 나중의 10시간보다 지금의 1시간이 더 치명적일 수 있다는 것을 간과하면 안된다.<br>
PMF를 아직 맞추지 못한 스타트업의 개발 Object는 "속도"가 되는 것이 일반적이고, 문제해결을 위해 존재하는 개발자라면, 이를 인정하고, 테스트 커버리지를 높이는 것이 정말 지금 필요한지를 꼭 고려해야한다.
물론 회사가 어느정도 PMF를 맞춰가며, 유저수도 늘어나고 Object가 "속도"에서 점점 "속도와 안정성"이 되어간다면, 테스트가 필요한 순간이 올 것으로 본다.<br>
배달 앱을 예로 들자면, 서버의 안정성에 문제가 생겨서 당장 전국 음식점 사장님들의 매출이 영향이 생긴다면, 이것은 비즈니스 자체에 큰 영향을 끼칠 수도 있으니말이다.<br>

### 그런데 지금 E2E를 고려하는 이유
글을 쓰는 시점에서 우리 비즈니스의 회원수는 이제 6만이 넘어가고 있고, 한달에 한번정도는 큰 에러들이 생긴다.<br>
예를 들자면, 회원가입이나 결제와 같은 "전환율"에 직접적인 영향을 미치는 부분에서 에러들이 종종 생긴다.<br>
그렇다고 지금 몇시간을 들여서 당장의 PMF맞추기를 후순위로 하고 테스트 코드를 짜는 것은 현실적으로 어려움이 있다.<br>
우리 비즈니스는 다행인지 다행이 아닌지, 한달에 한번꼴로 안정성에 큰 문제가 생기는 것으로 누군가에게 심각한 손해를 끼칠 정도의 일이 생기는 비즈니스는 아니기 때문이다.<br>
그래서 우리는 리스크를 감수하고 아직도 끊임없이 사용자가 불편함을 느끼는 직접적인 부분들을 계속 찾아나가고, 가설을 세우고, 솔루션을 찾고, 개발을 하고 있다.<br>
<br>
근데 지금 이런 글을 쓰면서 E2E테스트를 고려하는 이유는 당장 시간이 생겼다는 이유와, 이틀 전에 핵심적인 페이지의 오류를 하루동안 발견을 못했던 이슈가 있었기 때문이다.<br>
시간이 난 이유는 회사입장에서 다음 문제에 대한 솔루션을 계획하기까지 시간이 필요하기 때문이고, 오류를 하루동안 발견하지 못했던 것은 당연하지만 테스트 및 모니터링 시스템이 없기 때문이다.<br>
그래서 잠깐 생긴 이 시간을 이용하여 큰 에러라도 감지할만한 시스템을 만들면 좋지않을까를 생각해서 전에 라쿠텐 개발자님의 E2E 테스트 이야기가 생각나서 리서치를 해보는 중이다.<br>
내가 생각하는 시나리오는, E2E테스트 시스템을 도입하고 점진적으로 회원가입, 로그인 등의 시나리오를 하나씩 구축하며 단시간 내에 적어도 사용자의 기본적인 플로우는 보장하도록 하는 것이다.<br>
api명세가 바뀌고 프론트 컴포넌트들의 스펙이 계속 바뀌는 이 시점에서 그나마 E2E 테스트가 변동성에 대응하기 쉬우며 직접적일 것으로 판단했다.<br>
나머지 유닛 테스트, 통합 테스트 등은 시리즈 A에서 B는 되어야 제대로 테스트 커버리지를 고려하면서 도입할 수 있지 않을까싶다... 