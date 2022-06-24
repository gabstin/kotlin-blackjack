# kotlin-blackjack

## step 1 코틀린 DSL 실습

```
introduce {
  name("김승갑")
  company("kakao")
  skills {
    soft("A passion for problem solving")
    soft("Good communication skills")
    hard("Kotlin")
  }
  languages {
    "Korean" level 5
    "English" level 3
  }
}
```

### 구현 목록

- [x] Person 객체를 생성한다.
- [x] Person은 name 정보를 갖으며 DSL로 생성한다.
- [x] Person은 company 정보를 갖으며 DSL로 생성한다.
- [x] Person은 skills 정보를 갖으며 DSL로 생성한다.
    - [x] Skill에는 hard, soft 정보로 구성하며 DSL로 생성한다.
- [x] Person은 languages 정보를 갖으며 DSL로 생성한다.
    - [x] languages는 operator로 Subject, Level 정보를 추가한다.

## step 2 🚀블랙잭

### 기능 요구사항

- 블랙잭 게임을 변형한 프로그램을 구현한다. 블랙잭 게임은 딜러와 플레이어 중 카드의 합이 21 또는 21에 가장 가까운 숫자를 가지는 쪽이 이기는 게임이다.
- 카드의 숫자 계산은 카드 숫자를 기본으로 하며, 예외로 Ace는 1 또는 11로 계산할 수 있으며, King, Queen, Jack은 각각 10으로 계산한다.
- 게임을 시작하면 플레이어는 두 장의 카드를 지급 받으며, 두 장의 카드 숫자를 합쳐 21을 초과하지 않으면서 21에 가깝게 만들면 이긴다. 21을 넘지 않을 경우 원한다면 얼마든지 카드를 계속 뽑을 수 있다.

### 도메인

- Denomination : Ace, 2~9, Jack, Queen, King 으로 구성된 카드 정보
- Suit : 하드, 클로버, 스페이드, 다이아몬드로 구성된 카드 정보
- Card : 카드는 suit, denomination 으로 구성한다.
- Deck : Denomination, Suit 조합으로 구성된 카드 셋트
- GameRule : 게임과 연관된 정보
- Player : 게임을 진행할 플레이어
- Players : 게임을 진행할 복수의 플레이어
- ScoreCalculator : 카드의 점수 계산

### 설계

- InputView 에서 게임에 참여할 플에이어 정보를 입력받는다.
- 플레이어는 이름과 카드 정보를 갖는다.
- 카드는 미리 52개를 생성하여 랜덤한 순서로 저장한다.
- 카드를 분배할때는 미리 저장된곳에서 분배된 카드를 삭제하며, 대신 플레이어 카드 정보에 기록한다.
- 초기 2장의 카드를 플레이어에게 분배한다.
- 입력받은 카드 정보를 출력한다.
- 플레이어에게 다음 카드를 받을지 물어본다.
- 플레이어가 다음 카드 받기를 희망하면 카드를 뽑아 플에이어 카드 정보에 기록한다.
- 플에이어가 다음 카드 받기를 희망하지 않으면 스코어를 계산하고 종료한다.

### TDD

- [x] 플레이어는 이름과 카드 정보를 갖는다.
- [x] 플레이어는 카드 정보를 추가할 수 있다.
- [x] 플레이어 1급 콜렉션은 다수의 플레이어 정보를 갖는다.
- [x] Denomination 은 카드 이름과 스코어를 갖는다.
- [x] Suit 는 카드이름을 갖는다.
- [x] 카드는 suit, denomination 정보를 갖는다.
- [x] 카드는 suit, denomination 을 조합하여 실제 카드 이름을 생성한다.
- [x] Deck 은 52개의 카드로 구성되어야 한다.
- [x] Deck 은 카드를 분배할 수 있으며 분배된 카드는 Deck 에서 제거된다.
- [x] ScoreCalculator 는 카드 목록을 전달 받아 total 점수를 계산한다.

## 3단계 - 블랙잭(딜러)

### 기능 요구사항

- 블랙잭 게임을 변형한 프로그램을 구현한다. 블랙잭 게임은 딜러와 플레이어 중 카드의 합이 21 또는 21에 가장 가까운 숫자를 가지는 쪽이 이기는 게임이다.
- 카드의 숫자 계산은 카드 숫자를 기본으로 하며, 예외로 Ace는 1 또는 11로 계산할 수 있으며, King, Queen, Jack은 각각 10으로 계산한다.
- 게임을 시작하면 플레이어는 두 장의 카드를 지급 받으며, 두 장의 카드 숫자를 합쳐 21을 초과하지 않으면서 21에 가깝게 만들면 이긴다. 21을 넘지 않을 경우 원한다면 얼마든지 카드를 계속 뽑을 수 있다.
- 딜러는 처음에 받은 2장의 합계가 16이하이면 반드시 1장의 카드를 추가로 받아야 하고, 17점 이상이면 추가로 받을 수 없다.
- 딜러가 21을 초과하면 그 시점까지 남아 있던 플레이어들은 가지고 있는 패에 상관 없이 승리한다.
- 게임을 완료한 후 각 플레이어별로 승패를 출력한다.

### 도메인 파악

- 플레이어와 함께 딜러는 게임에 참여한다.
- 딜러, 플레이어 모두 2개의 카드를 받는다.
- 딜러는 2개의 카드의 합이 16이하이면 추가로 카드를 받아야한다.
- 승,무승부,패를 판단할때 딜러가 21을 초과한 경우 플레이어는 "승"이되며 딜러는 "패"가된다.

### TDD

- [x] Dealer, Player의 상위 개념인 Participant을 생성하며 카드를 받을 수 있는지 확인하는 추상 메소드를 갖는다.
- [ ] Dealer, Player은 서로 다른 조건으로 카드을 받을 수 있는지 판단한다.
- [ ] Score을 이용하여 plus, equal, greater을 판단한다.
- [ ] Participant는 두개의 Score을 비교하여 승, 패, 무승부를 판단한다.
- [ ] View 기능을 구현한다.
