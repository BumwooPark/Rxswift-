## RXSwift 

Just 
: 하나의 엘리먼트를 관찰하는 관찰자를 만듬

of 
: 고정된 엘리먼트를 관찰하는 관찰자를 만든다

from 
: Array나 dictionary등의 시퀸스를 관찰하는 관찰자를 만든다 

create
: custom 시퀀스 관찰자를 만든다.  enum 타입의 .next , .error , .complete를 설정하면 된다.

range 
: 범위에 대하나 시퀀스의 요소를 하나씩 뽑아낸다 .

repeatElement
:    
```
Observable.repeatElement("🔴")
        .take(3)
        .subscribe(onNext: { print($0) })
        .disposed(by: disposeBag)
```

이런식으로 가능 


deferred
: lazy initialize observable 생성자로 subscribe가 발생할때 observable이 생성된다.
 일반적인 lazy라고 생각하자.

doOn
: 단일 특정 이벤트를 인터셉트한다 observable에서 


## publishSubject

observer와 observable두개의 역할을 하는 브릿지 또는 프록시의 종류  


PublishSubject
: subscribe된 시점 이후부터 해당 observer에게 이벤트들을 전달한다 .

```
   let disposeBag = DisposeBag()
    let subject = PublishSubject<String>()
    subject.onNext("1")
    subject.addObserver("1").disposed(by: disposeBag)
    subject.onNext("🐶")
    subject.onNext("🐱")
```

결과

```
--- PublishSubject example ---
Subscription: 1 Event: next(🐶)
Subscription: 1 Event: next(🐱)
Subscription: 1 Event: next(🅰️)
Subscription: 2 Event: next(🅰️)
```


ReplySubject
: 정해진 버퍼의 사이즈 만큼 가장 나중에 들어온 이벤트를 새로운 subscriber에게 전달한다 

```
let disposeBag = DisposeBag()
    let subject = ReplaySubject<String>.create(bufferSize: 2)
  
  
    subject.addObserver("1").disposed(by: disposeBag)
    subject.onNext("🐶")
    subject.onNext("🐱")
    
    subject.addObserver("2").disposed(by: disposeBag)
    subject.onNext("🅰️")
    subject.onNext("🅱️")
  
    subject.addObserver("3").disposed(by: disposeBag)
    subject.onNext("1")
```

결과 

```
Subscription: 1 Event: next(🐶)
Subscription: 1 Event: next(🐱)
Subscription: 2 Event: next(🐶)
Subscription: 2 Event: next(🐱)
Subscription: 1 Event: next(🅰️)
Subscription: 2 Event: next(🅰️)
Subscription: 1 Event: next(🅱️)
Subscription: 2 Event: next(🅱️)
Subscription: 3 Event: next(🅰️)
Subscription: 3 Event: next(🅱️)
Subscription: 1 Event: next(1)
Subscription: 2 Event: next(1)
Subscription: 3 Event: next(1)
```





