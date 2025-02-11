<p>오늘은 외부 API에 요청과 응답을 주고받아야 할 일이 생겨서 여러 방법을 찾아보았습니다. 처음 해보는 것이기 때문에 열심히 찾아보니 여러 방법들(HttpURLConnection, HttpClient, WebClient, OpenFeign 등)이 있었는데 저는 Spring boot Web에서 제공하는 <strong>RestTemplate</strong>를 사용하기로 했습니다. 이유는 가장 많이 사용되는 방법으로 알려져 있기 때문입니다.</p>
<p><em>이 RestTemplate는 Spring Framework 3.x 버전에서 처음으로 등장했습니다. 이후 Spring Framework 5.x 버전에서 WebClient가 등장해서 RestTemplate이 Depreacted(폐기...)된다는 말이 있었는데, 이는 찌라시라고... 현재까지도 잘 사용 되는 중입니다.</em></p>
<h2 id="resttemplate이란">RestTemplate이란?</h2>
<p>RestTemplate은 HTTP 통신을 위한 도구입니다. RESTful API 웹 서비스와의 상호작용을 쉽게 해주는 스프링 프레임워크의 클래스입니다.
JSON이나 XML 등의 데이터 형식으로 통신하며 다양한 HTTP 메서드를 사용합니다.
<strong>동기식 방식</strong>으로, 요청을 보낸 뒤 응답을 받을 때까지 블로킹되며, 요청과 응답이 완료되기 전까지 다음 코드로 진행되지 않습니다. 원격 서버와 통신할 때는 응답을 기다리는 동안 대기하는 방식으로 진행됩니다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/ab3e3db5-936f-4e31-bf69-83ba3880e5f3/image.png" /></p>
<h3 id="resttemplate-특징">RestTemplate 특징</h3>
<h4 id="1-동기식-요청">1. 동기식 요청</h4>
<ul>
<li>요청을 보내고 응답을 받기까지 블로킹 상태(결과 반환까지 대기하는 상태)로 있는다. unblocked 상태가 되면 다음 요청에 대해 처리를 수행합니다.</li>
<li><em>반면 비동기식 요청(Asynchrouse Request)은 webflux에서 제공하는 논 블로킹 요청을 수행하며 요청을 보내고 결과가 반환되지 않더라도 다른 작업을 수행 가능합니다. 이는 앞서 말한 WebClient의 방식입니다.</em><h4 id="2-http-메서드에-따른-메서드-제공">2. HTTP 메서드에 따른 메서드 제공</h4>
</li>
<li>RestTemplate의 경우, HTTP Method에 따른 메서드를 제공합니다.</li>
</ul>
<table>
<thead>
<tr>
<th align="left">메서드</th>
<th>HTTP 메서드</th>
<th>반환 타입</th>
<th align="left">설명</th>
</tr>
</thead>
<tbody><tr>
<td align="left">getForObject()</td>
<td>GET</td>
<td>Object</td>
<td align="left">GET 요청에 대한 결과를 Object로 반환</td>
</tr>
<tr>
<td align="left">getForEntity()</td>
<td>GET</td>
<td>ResponseEntity</td>
<td align="left">GET 요청에 대한 결과를 ResponseEntity로  반환</td>
</tr>
<tr>
<td align="left">postForLocation()</td>
<td>POST</td>
<td>URI</td>
<td align="left">POST 요청에 대한 결과로 헤더에 저장된 URI를 반환</td>
</tr>
<tr>
<td align="left">postForObject()</td>
<td>POST</td>
<td>Object</td>
<td align="left">POST 요청에 대한 결과를 Object로 반환</td>
</tr>
<tr>
<td align="left">postForEntity()</td>
<td>POST</td>
<td>ResponseEntity</td>
<td align="left">POST 요청에 대한 결과를 ResponseEntity로  반환</td>
</tr>
<tr>
<td align="left">put()</td>
<td>PUT</td>
<td>void</td>
<td align="left">PUT 요청 실행</td>
</tr>
<tr>
<td align="left">patchForObject()</td>
<td>PATCH</td>
<td>Object</td>
<td align="left">PATCH 요청을 실행하고 결과를 Object로 반환</td>
</tr>
<tr>
<td align="left">delete()</td>
<td>DELETE</td>
<td>void</td>
<td align="left">DELETE 요청 실행</td>
</tr>
<tr>
<td align="left">headForHeaders()</td>
<td>HEADER</td>
<td>HttpHeaders</td>
<td align="left">헤더 정보를 추출하고 HTTP HEAD 메서드를 사용</td>
</tr>
<tr>
<td align="left">optionForAllow()</td>
<td>OPTIONS</td>
<td>Set</td>
<td align="left">지원하는 HTTP 메서드를 추출</td>
</tr>
<tr>
<td align="left">exchange()</td>
<td>any</td>
<td>ResponseEntity</td>
<td align="left">헤더를 생성하고 모든 요청 방법 허용</td>
</tr>
<tr>
<td align="left">execute()</td>
<td>any</td>
<td>T</td>
<td align="left">요청/응답 콜백 수정</td>
</tr>
</tbody></table>