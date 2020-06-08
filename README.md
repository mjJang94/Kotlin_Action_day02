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

자바처럼 코틀린에서도 클래스를 abstarct
