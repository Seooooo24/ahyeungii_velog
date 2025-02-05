<p>이번에 커서 기반 페이지네이션을 구현할 일이 생겨 따라하다 보니, Page와 Slice 같은 생소한 타입을 보게 되었습니다. 그리하여 이번에는 이 둘에 대해서 알아보겠습니다.</p>
<h2 id="jpa-paging">JPA Paging</h2>
<p>먼저 JPA에서 Paging이란 무엇을 의미할까요?
DB에 저장된 자원을 페이지로 나누어주는 것입니다.
자원을 일정 개수(기준)로 분류하고, 분류된 부분들 중 어떤 부분을 보낼 수 있게 해주는 것이 JPA Paging입니다.</p>
<p>페이징 처리를 위해서는 한 페이지 당 보여줄 데이터의 개수, 그리고 몇 번째 페이지에 대한 정보가 필요합니다. JPA에서는 이 정보들을 Pageable 객체를 이용해서 받습니다. 이 Pageable 객체를 JpaRepository에 전달하여 페이징 처리를 지원합니다.</p>
<h3 id="pageable">Pageable</h3>
<p>Pageable 객체는 PageRequest의 정적 팩토리 메소드인 of를 통해 구현할 수 있습니다. PageRequest의 of 메소드는 현재 페이지를 의미하는 page, 그리고 페이지 당 보여줄 데이터를 받을 size를 필수 파라미터로 받고, 추가적으로 sort 방식을 파라미터로 받을 수 있게 되어 있습니다.</p>
<h3 id="slice">Slice</h3>
<p>Slice 인터페이스 내부를 살펴보면 이러한 구조로 이루어져 있습니다.</p>
<pre><code>package org.springframework.data.domain;

import java.util.List;
import java.util.function.Function;

import org.springframework.core.convert.converter.Converter;
import org.springframework.data.util.Streamable;

public interface Slice&lt;T&gt; extends Streamable&lt;T&gt; {

    int getNumber(); // 현재 페이지

    int getSize(); //페이지 크기

    int getNumberOfElements(); // 현재 페이지에 조회한 데이터 개수

    List&lt;T&gt; getContent(); // 현재 페이지에 조회한 데이터 

    boolean hasContent(); // 현재 페이지에 데이터가 있는지 여부 

    Sort getSort(); // 정렬 여부 

    boolean isFirst(); // 첫 번째 페이지인지 여부

    boolean isLast(); // 마지막 페이지인지 여부

    boolean hasNext(); // 다음 페이지가 있는지 여부

    boolean hasPrevious();  // 이전 페이지가 있는지 여부 

      // 페이지 요청 정보
    default Pageable getPageable() {
        return PageRequest.of(getNumber(), getSize(), getSort());
    }

    // 다음 페이지 정보
    Pageable nextPageable();

    // 이전 페이지 정보
    Pageable previousPageable();


    &lt;U&gt; Slice&lt;U&gt; map(Function&lt;? super T, ? extends U&gt; converter);
    default Pageable nextOrLastPageable() {
        return hasNext() ? nextPageable() : getPageable();
    }

    default Pageable previousOrFirstPageable() {
        return hasPrevious() ? previousPageable() : getPageable();
    }
}</code></pre><h3 id="page">Page</h3>
<p>Page 인터페이스는 Slice를 상속받습니다. 상속받은 메소드 외에 getTotalPages(), getTotal Elements() 메소드를 추가적으로 가집니다.</p>
<pre><code>public interface Page&lt;T&gt; extends Slice&lt;T&gt; {

    static &lt;T&gt; Page&lt;T&gt; empty() {
        return empty(Pageable.unpaged());
    }

    static &lt;T&gt; Page&lt;T&gt; empty(Pageable pageable) {
        return new PageImpl&lt;&gt;(Collections.emptyList(), pageable, 0);
    }

    int getTotalPages();  // 전체 페이지 개수

    long getTotalElements(); // 전체 데이터 개수

    &lt;U&gt; Page&lt;U&gt; map(Function&lt;? super T, ? extends U&gt; converter);
}</code></pre><h3 id="page와-slice-비교">Page와 Slice 비교</h3>
<p>Slice는 전체 데이터 개수와 전체 페이지 수를 조회하지 않을 때 사용합니다. 전체 데이터를 확인하지 않고, 다음 Slice가 있는지 없는지 여부만을 판단하므로 Page보다 성능이 좋습니다.</p>
<p>일반적인 페이징 처리(오프셋 기반 페이지네이션)를 하기 위해서는 Page를 이용해야 하겠고, 무한 스크롤 방식(커서 기반 페이지네이션)으로 페이징 처리를 하기 위해서는 Slice를 이용하면 성능면에서 유리할 것 같습니다.</p>
<p>요구 사항에 따라서 사용하면 될 것 같습니다.</p>
<p>저는 커서 기반 페이지네이션을 구현 중이었는데, 예제를 따라하다 보니 Slice 타입을 사용하게 돼서 조사하게 되었습니다. 이런 차이점이 있었다는 걸 알게 돼서 유익했습니다!</p>
<p><strong>참고</strong>
<a href="https://zayson.tistory.com/entry/Spring-Data-JPA%EC%9D%98-Page%EC%99%80-Slice">https://zayson.tistory.com/entry/Spring-Data-JPA%EC%9D%98-Page%EC%99%80-Slice</a></p>