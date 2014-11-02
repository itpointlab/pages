# Controller Hacking

## About 

스마트폰 헤드폰에 기본적으로 있는 콘트롤러를 해킹해 보았다. 기본 목표는 아래와 같다.

- 콘트롤러의 작동구조를 이해하고 이를 재구현 한다.
- 아두이노를 통해 버튼을 온/오프할 수 있다.

![](https://farm8.staticflickr.com/7551/15503108118_e4c19d6c67.jpg)

- black -> signal (MIC)
- orange -> ground

![](https://farm6.staticflickr.com/5597/15069131083_4928fffd5e.jpg)

빵판에 필요한 부품들을 적당히 배치한다. 재미난 점은 반드시 마이크가 있어야 작동한다는 것이다. 콘트롤러의 작동원리는 단순히 각 버튼간의 저항값의 차이로 작동한다.

![](https://farm4.staticflickr.com/3955/15686571661_baecc6e336.jpg)

아두이노로 버튼을 콘트롤 하기 위해서 4N35를 이용해 보았으나 용량의 문제인건지 작동하지 않는다. 외부 전원으로 따로 테스트하면 정상 작동한다.

![](https://farm4.staticflickr.com/3941/15503677390_1d929049fc.jpg)

[](http://www.youtube.com/watch?v=XYHJKEU1-zs)

## Hardware

![](http://www.george-smart.co.uk/w/images/thumb/a/ad/HTCHeadsetSchematic.png/600px-HTCHeadsetSchematic.png)

http://www.george-smart.co.uk/wiki/HTC_Headphones

4N25 Optocoupler

![](https://farm6.staticflickr.com/5607/15493672778_099e2efa99.jpg)

![](http://wiring.org.co/learning/basics/imgs/Optocoupler4N35.png)

## Sketch

```cpp
#include <Bounce2.h>

#define BUTTON_PIN 2
#define LED_PIN 8

Bounce debouncer = Bounce();
unsigned long buttonPressTimeStamp;
char *result = "value : ";

// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  debouncer.attach( BUTTON_PIN );
  debouncer.interval(5);
}

// the loop routine runs over and over again forever:
void loop() {
  debouncer.update();
  if ( debouncer.fell() ) {
    Serial.println( millis()-buttonPressTimeStamp );
    buttonPressTimeStamp = millis();
  }
  int value = debouncer.read();
  Serial.print(result);
  Serial.println(value);
  if ( value == LOW ) {
    digitalWrite(LED_PIN, HIGH );
  } 
  else {
    digitalWrite(LED_PIN, LOW );
  }
}
```

<div class="btn btn-primary" onclick="location.href='https://github.com/itpointlab/controller-hacking'">Download on GitHub</div>

## Issue

- 아이폰과 안드로이드 그라운드와 시그널 위치가 달라 혼용 할 수 없음.
- 아두이노에서 온/오프 스위칭을 하기위해 옵트커플러를 사용 했으나 작동하지 않음.

## Link

http://www.elechouse.com/elechouse/index.php?main_page=product_info&cPath=90_92&products_id=2199
http://fritzing.org/projects/optocoupler
http://www.slideshare.net/wolfpaulus/android-arduino-and-the-headphone-jack

[gimmick:Disqus](itpointlab-github-io)
