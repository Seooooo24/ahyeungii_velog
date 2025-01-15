<p>Spring을 이용하면서 개발하다 보면 Date를 저장해야할 일이 많다. 이때 신경써야할 것이 Date Format이다. </p>
<p>이 Date 포매팅을 해주는 어노테이션이 바로 <code>@DateTimeFormat(pattern = &quot;yyyy-MM-dd&quot;)</code>이다. 이는 스프링 어노테이션이다.</p>
<p>이 어노테이션은 날짜, 시간 문자열을 자바의 <code>java.util.Date</code>, <code>java.time.LocalDate</code>, <code>java.time.LocalTime</code>, <code>java.time.LocalDateTime</code>, <code>java.util.Calendar</code>등의 객체로 변환해준다. </p>
<blockquote>
<p>그러나, <code>DateTimeFormat</code>은 @RequestBody와 @ResponseBody에서는 동작하지 않는다. 이 둘에는 Jackson 라이브러리(Java 제공 JSON 파싱 라이브러리)가 사용되기 때문이다. Spring에서 Jackson을 의존하기 때문에 Spring은 Jackson을 알고 있지만, Jackson은 Spring을 모른다. </p>
</blockquote>
<p>대신 @RequestBody나 @ResponseBody에서는 <code>@JsonFormat</code>을 사용한다. 이 어노테이션은 shape, pattern, timezone을 필요로 한다.</p>