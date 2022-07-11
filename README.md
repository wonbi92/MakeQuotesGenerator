# Wonbi's Study!

## QuotesGenerator 만들기 실습

1. UIKit
- 화면이나 UI, 앱의 동작 등 기능 구현을 주로 담당하는 프레임워크
- 화면을 그리는 `View`, 스크린에 View를 출력하는 `Window`, Window와 View를 연결해주는 `ViewController`등이 대표적인 객체들이다.
- UIKit은 내부적으로 Foundation Framework를 import하고 있기 때문에 UIKit을 사용하면 Foundation도 사용할 수 있다.
- 결론적으로, UIKit은 UI(화면)과 관련된 기능(Window, View 등)을 담당, Foundation은 UI를 제외한 앱의 중심 기능(자료형, 네트워킹, 타이머 등)을 담당한다.

2. AutoLayout
- 제약 조건(Constraints)에 따라 뷰 계층 구조에 있는 모든 뷰의 크기와 위치를 동적으로 지정하는 것.
- 아이폰의 다양한 해상도 비율에 대응하기 위해 만들어진 개념이다.
- 명언 생성기의 화면을 구성할 때, AutoLayout을 통해 각 View와 화면과의 간격, View와 View사이의 간격, 정렬 등을 지정하여서 화면을 만들었다.

3. IBOutlet & IBAction
- IBOutlet은 스토리보드에 등록한 UIObjet를 코드의 변수로 접근할 수 있게 만들어주는 역할.
- IBAction은 버튼과 연결시켜 이벤트를 처리하는 함수는 만들어 주는 역할.
- 이번에는 IBOutlet을 이용해서, 명언 라벨과 이름 라벨 변수를 뷰컨트롤러 클래스에 선언해주었다.
```Swift
    @IBOutlet weak var quoteLabel: UILabel!
    @IBOutlet weak var nameLabel: UILabel!
```
- 마찬가지로 IBAction를 이용해서, 명언 생성 버튼을 눌렀을 때 정의한 함수를 호출되게 만들었다.
```Swift
    @IBAction func tapQuoteGeneratorButton(_ sender: Any) {
        let random = Int.random(in: 0..<quotes.count)
        let quote = quotes[random]
        self.quoteLabel.text = quote.contents
        self.nameLabel.text = quote.name
    }
```
- 실습 강의에서는 `arc4random`이라는 명령어를 사용하여 난수를 만들었는데, 처음보는 명령어였다. 찾아보니 Swift4.2이전 버전에서 사용하던 랜덤으로 Double타입의 숫자를 뽑아주는 명령어였다. 
- Swift4.2이후 부터는 `Int.random(in: range)`를 통해 바로 Int타입의 난수를 뽑아줄 수 있었고, 나도 이 방법은 이미 알고 있었기 때문에 이걸로 실습을 진행하였다.
- 난수 범위 역시 고정값이 아니라 명언이 들어있는 배열 내부의 컨텐츠 갯수에 맞게 계속 변할 수 있도록 수정해주었디.

4. Content Hugging & Compression Resistance
- UILabel, UIButton와 같은 View의 속성, 이미지, 텍스트의 길이에 따라 크기가 결정되는 View가 있는데, 이런 View들은 다른 View들 사이에 걸린 제약에 의해 본래의 컨텐츠보다 더 늘어날 수도 줄어들 수도있다.
- 좀 더 쉽게 말해, 명언의 길이에 따라 명언 라벨의 크기는 계속 변하게 될 것이다. 
- 이 때 크기가 더 커지는, 늘어나는 것에 대해 저항하는 것을 Content Hugging, 크기가 작아지는, 줄어드는 것에 대해 저항하는 것을 Compression Resistance이라 한다.
- Content Hugging과 Compression Resistance은 우선순위를 가지고 있다. 서로 관계되어 있는 라벨들 사이에서 컨텐츠의 양에 따라 줄어들거나, 늘어나야 할 때 어떤 라벨을 우선적으로 늘이거나 줄일지 결정하는 것이다.
- 이는 1~1000사이의 값으로 우선순위를 정할 수 있다.
- Content Hugging과 Compression Resistance의 우선순위는 높을수록 자신의 크기를 유지하려 하고, 낮을수록 늘어나거나 줄어든다.
- 이번 명언 생성기의 경우 명언의 길이가 길면, 명언 라벨의 크기가 커져야 하므로 Content Hugging의 우선순위를 이름 라벨보다 작게 설정했다.
- 마찬가지로 이름 라벨은 명언의 길이가 엄청 길어지더라도 사라지면 안되기 때문에 Compression Resistance의 우선순위를 명언 라벨보다 높게 설정하였다.
