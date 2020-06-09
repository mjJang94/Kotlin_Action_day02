# open, final, abstract 변경자 : 기본적으로 final   
어떤 클래스가 자신을 상속하는 방법에 대해 정확한 규칙을 제공하지 않는다면(어떤 메소드를 어떻게 오버라이드해야 하는지 등) 그 클래스의 클라이언트는 기반 클래스를 작성한 사람의 의도와 다른 방식으로 메소드를 오버라이드할 위험이 있다.    
하위 클래스에서 오버라이드하게 의도된 클래스와 메소드가 아니라면 모두 final로 만드는 것이 더 낫다. 자바의 클래스와 메소드는 기본적으로 상속에 대해 개방적이지만 코틀린의 클래스와 메소드는 기본적으로 final을 유지한다. 그러므로 어떤 클래스의 상속을 허용하려면 클래스 앞에 open 변경자를 붙여야 한다. 또한 오버라이드를 허용하고 싶은 메소드나 프로퍼티의 앞에도 open 변경자를 붙여줘야 한다.

<pre>
<code>
open class RichButton : Clickable {
  fun disable() {}    //----- 기본속성이 final, 하위 클래스에서 오버라이드 불가능
  open fun animate() {} //----- 하위 클래스에서 메소드 오버라이드 가능
  override fun click() {} //----- 이 함수는 열려있는 메소드를 오버라이드
}
</code>
</pre>

만약 오버라이드하는 메소드의 구현을 하위 클래스에서 오버라이드하지 못하게 금지하려면 오버라이드하는 메소드에 fianl을 명시해준다.

<pre>
<code>
open class RichButton : Clickable {
 final override fun click() {}
}
</code>
</pre>

자바처럼 코틀린에서도 클래스를 abstarct로 선언할 수 있는데 abstract로 선언한 추상 클래스는 인스턴스화할 수 없다.   
추상 멤버는 항상 열려있으므로 멤버 앞에 따로 open을 명시해줄 필요는 없다.

<pre>
<code>
abstract class Animated {
  
  abstract fun animate()
  
  open fun stopAnimating() { //-----
                                    |
  }                                 |----- 추상 클래스에 속했더라도 비추상 함수는 기본적으로 파이널이지만 원한다면 open으로 오버라이드를 허용할 수 있다.
                                    |
  fun anumateTwice() { //-----------
  
  }
  
}
</code>
</pre>

### 키워드 정리
- fianl (오버라이드할 수 없음)
- open (오버라이드할 수 있음)
- abstract (반드시 오버라이드해야 함)
- override (상위 클래스나 상위 인스턴스의 멤버를 오버라이드하는 중)

# 가시성 변경자: 기본적으로 공개
가시성 변경자는 코드 기반에 있는 선언에 대한 클래스 외부 접근을 제어한다. 어떤 클래스의 구현에 대한 접근을 제한함으로써 그 클래스에 의존하는 외부 코드를 꺠지 않고도 클래스 내부 구현을 변경할 수 있다.   
기본적으로 java와 같은 public, protected, private 변경자가 있다. 하지만 코틀린의 특이점은 아무 변경자가 없는 경우 선언은 모두 public이 되어버린다.   
자바의 기본 가시성인 패키지 개념이 코틀린에는 없다. 코틀린에서는 그저 네임스페이스를 관리하기 위한 용도로만 사용한다. 이러한 패키지 개념의 부재로인한 대안으로 코틀린에는 internal이라는 가시성 변경자를 도입했다. internal은 **모듈 내부에서만 볼 수 있음** 이라는 특징을 가진다.
그리고 자바와 코틀린의 protected는 다르다. java에서는 같은 패키지 안에서 protected 멤버에 접근할 수 있지만, 코틀린에서는 그렇지 않다. protected 멤버는 오직 어떤 클래스나 그 클래스를 상속한 클래스 안에서만 보인다.

<pre>
<code>
internal open class TalkativeButton : Focusable {
  private fun yell() = println("Hey!")
  protected fun whisper() = println("Let's a talk!")
}

fun TalkativeButton.giveSpeech() { //------ public 함수인 giveSpeech 안에서 그보다 가시성이 더 낮은 타입인 TalkativeButton을 참조하지 못하게 함

  yell() //----- TalkativeButton의 private 멤버라서 오류
  
  whisper() //----- TalkativeButton의 protected 멤버라서 오류
}
</code>
</pre>
