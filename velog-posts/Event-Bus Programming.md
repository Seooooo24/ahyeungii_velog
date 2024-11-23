<h3 id="event-bus란">Event-Bus란?</h3>
<ul>
<li>외부 프로세스와의 연동을 위해 <strong>이벤트</strong>를 발생하여 통신하는 방법
버스는 이벤트가 다니는 통로라고 보면 된다. 이벤트를 발생시킨 뒤 그걸 이벤트 버스를 통해서 다른 프로세스에 전달해주고, 그 이벤트를 받은 프로세스가 결괏값을 다시 리턴해주는 방식이다.</li>
</ul>
<h3 id="event-bus의-특징">Event-Bus의 특징</h3>
<ul>
<li><strong>이벤트 기반으로 통신하면 시스템의 변경이 상대적으로 쉽다</strong>. 클라이언트-서버 프로그래밍 같은 경우는 직접 서버와 연결되어 있기 때문에 서버의 위치를 정확하게 지정해주어야 한다. 그러나 이벤트-버스 프로그래밍은 버스에 연결되어 있는 다른 프로세스들로 이벤트를 전달하기가 쉬우므로 훨씬 쉽게 시스템 변경이 가능하다.</li>
<li>다만, 이벤트가 명확히 전송되었는지 알 수가 없으며, 응답 시간을 예측하기가 매우 어렵다. (Non-Deterministic)</li>
<li>정리하자면, 변경은 쉬운데 결정이 어렵다.</li>
</ul>
<h3 id="event-bus의-종류">Event-Bus의 종류</h3>
<h4 id="implicit-invocation">Implicit Invocation</h4>
<ul>
<li>변경 정보를 전달할 대상을 명확히 알지 않고 정보를 전달한다.</li>
<li>하나를 뿌리면 불특정 다수가 모두 전달 받는다. 특정 하나한테 보내는 게 아니라, 브로드캐스팅과 같은 것이다. 받고 싶은 사람만 받아서 쓰는 것과 마찬가지! </li>
<li>변경이 훨씬 쉽다. 대신 모두에게 보내기 때문에 예측이 어렵다!
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/455c1fd1-464f-44c5-91f2-2bf43e125b69/image.png" /></li>
</ul>
<h4 id="explicit-invocation">Explicit Invocation</h4>
<ul>
<li>변경 정보를 전달한 대상을 명확히 알고 변경 정보를 전달한다.</li>
<li>클라이언트-서버 방식과 비슷하다. 특정한 받을 사람을 지정해서 뿌리는 것이다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/c185f121-b614-4ce0-82fd-934680ea5cec/image.png" /></li>
</ul>