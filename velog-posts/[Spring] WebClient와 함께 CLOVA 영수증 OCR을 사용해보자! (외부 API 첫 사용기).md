<p> 이번 팀 프로젝트에서 사용자에게 영수증 인식 기능을 제공하고자 했기에 OCR API를 사용하기로 했습니다. 많은 OCR API들 중 클로바 OCR이 압도적인 성능을 자랑한다는 글을 보고 클로바 OCR을 사용하기로 결정했습니다.
 외부 API를 사용해보는 것이 처음이었기 때문에 처음에 갈피를 못 잡았습니다... 그런데 막상 해보니까 쉽게쉽게 진행돼서 좋았습니다.</p>
<h2 id="api-사용-준비하기">API 사용 준비하기</h2>
<h3 id="네이버-클라우드-플랫폼-api-이용-신청하기">네이버 클라우드 플랫폼 API 이용 신청하기</h3>
<p> 먼저, 네이버 클라우드 플랫폼의 OCR 페이지에 들어가자. (<a href="https://www.ncloud.com/product/aiService/ocr">https://www.ncloud.com/product/aiService/ocr</a>)</p>
<p> 들어가면 이용 신청을 할 수 있는 버튼이 있다. 누르면 계정을 생성하라고 하는데 회원가입 하고 결제수단 등록하면 된다. 그리고 결제수단을 등록하면 100,000캐시를 준다. 이 캐시를 사용해서 서비스 요금을 낼 수 있는 듯. 
 <img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/6028e718-69aa-4e82-a298-81a341676e59/image.png" /></p>
<p>이용 신청 버튼을 누르면 내 콘솔에 들어갈 수 있다. 대시보드가 기본으로 뜨는데 우리는 좌측 메뉴의 [Services-AI Services-CLOVA OCR]의 경로로 들어가면 CLOVA OCR에 대한 설명이 나온다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/39574a6c-ecd9-40e9-9f3a-50a8a09f64b8/image.png" /></p>
<p>아래에 이용 신청 버튼이 또 나온다. 이걸 눌러주면 서비스 이용 약관이 나온다. 동의하고 신청해보자.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/5f14e71d-83ac-498a-8657-a42f57d9179c/image.png" /></p>
<p>이용 신청이 완료됐다. 새로운 도메인 생성 창이 나오는데 새로운 도메인을 여기서 생성한다. (창을 종료한 경우 일반/템플릿 도메인 생성 버튼을 눌러 생성해보자)
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/6696a17a-43a7-4ffc-b6b3-4c18a7ed694d/image.png" /></p>
<p>도메인명, 도메인 코드 등 적어서 생성하자
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/41499add-de01-470e-8169-44c48df59de9/image.png" /></p>
<p>고급 플랜이나 특화 모델을 사용할 경우에는 유지 비용이 나올 수 있다.</p>
<h3 id="영수증-특화-모델-사용하려면">영수증 특화 모델 사용하려면?</h3>
<p>도메인 생성 옆의 '특화 모델 설정' 버튼을 눌러준다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/91899f1c-d03c-43b5-9f7b-35ff0816b98e/image.png" /></p>
<p>이렇게 다양한 종류의 특화 모델이 있는 걸 볼 수 있는데, '특화 모델 신청' 버튼을 누르면 이렇게 사용 신청서를 작성할 수 있다. 간단하게 작성해서 제출한다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/b7adca69-77a8-4de6-9013-95232086215b/image.png" /></p>
<p>제출하고 사용까지 승인을 기다려야 하므로 개발 전에 미리 신청해두는 것이 좋다.
승인이 되고 나면 위 사진 처럼 승인이 표시된다. 이후 도메인 생성 버튼을 누르고 위와 똑같이 생성하면 된다.</p>
<h3 id="api-gateway-연동">API Gateway 연동</h3>
<p>이러한 외부 API와 클라이언트를 연결하기 위해서는 API Gateway라는 개념이 필요하다.
다음으로는 API Gateway를 연동해보자. 우측의 API Gateway 연동 버튼을 클릭하면 이런 창이 뜬다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/c84184a5-b0c3-4658-81a9-bfb08f21bd31/image.png" />
시크릿 키를 생성받고, 자동 연동 버튼을 누른다.</p>
<p>눌렀을 때 API Gateway 이용 신청이 우선되어야 한다고 경고한다.
API Gateway는 내 콘솔의 [Services-Application Services-API Gateway] 경로로 들어가서 이용 신청이 가능하다.
둘 다 신청하면 생성이 완료된다.
<img alt="" src="https://velog.velcdn.com/images/ahyeungii/post/d4bdde8c-c1e2-463c-85a5-0c5752396c39/image.png" />
이렇게 하면 준비는 gateway 준비는 모두 끝났다!</p>
<h2 id="webclient-이용하여-요청-보내기">WebClient 이용하여 요청 보내기</h2>
<pre><code>
@Service
public class OcrServiceImpl implements OcrService{
    private static final String CLOVA_VERSION = &quot;V2&quot;;
    private static final String CLOVA_TYPE = &quot;receipt&quot;;

    private final WebClient webClient;

    @Value(&quot;${naver.service.base-url}&quot;)
    private String baseUrl;
    @Value(&quot;${naver.service.endpoint}&quot;)
    private String endpoint;
    @Value(&quot;${naver.service.secretKey}&quot;)
    private String secretKey;
    @Value(&quot;${spring.cloud.gcp.storage.bucket}&quot;)
    private String bucketName;

    public OcrServiceImpl(WebClient.Builder webClientBuilder) {
        this.webClient = webClientBuilder.baseUrl(baseUrl).build();
    }

    public OcrResponse sendOcrRequest(String contentType, String imagePath) throws IOException {
        Map&lt;String, Object&gt; imageData = setImageData(contentType, imagePath);
        Map&lt;String, Object&gt; requestBody = setRequestBody(imageData);

        return webClient.post()
                .uri(endpoint)
                .header(&quot;Content-Type&quot;, &quot;application/json&quot;)
                .header(&quot;X-OCR-SECRET&quot;, secretKey)
                .body(Mono.just(requestBody), Map.class)
                .retrieve()
                .bodyToMono(OcrResponse.class)
                .block();
    }

    private Map&lt;String, Object&gt; setImageData(String contentType, String imagePath)
            throws IOException {
        Map&lt;String, Object&gt; imageData = new HashMap&lt;&gt;();
        imageData.put(&quot;format&quot;, contentType);
        imageData.put(&quot;data&quot;, encodeImage(imagePath));
        imageData.put(&quot;name&quot;, CLOVA_TYPE);
        return imageData;
    }

    private Map&lt;String, Object&gt; setRequestBody(Map&lt;String, Object&gt; imageData) {
        Map&lt;String, Object&gt; requestBody = new HashMap&lt;&gt;();
        requestBody.put(&quot;version&quot;, CLOVA_VERSION);
        requestBody.put(&quot;requestId&quot;, createRequestId());
        requestBody.put(&quot;timestamp&quot;, System.currentTimeMillis());
        requestBody.put(&quot;images&quot;, new Map[]{imageData});
        return requestBody;
    }

    private String encodeImage(String imagePath) throws IOException {
        Storage storage = StorageOptions.getDefaultInstance().getService();
        Blob blob = storage.get(bucketName, imagePath);
        if (blob == null) {
            throw new FileNotFoundException(&quot;스토리지에서 파일을 찾을 수 없습니다.&quot;);
        }
        byte[] imageBytes =  blob.getContent();

        return Base64.getEncoder().encodeToString(imageBytes);
    }

    private String createRequestId() {
        return UUID.randomUUID().toString();
    }
}
</code></pre><p>OcrService 코드 전문이다.
webClient의 post() 메서드를 통해 헤더를 설정하고 Map&lt;String, Object&gt; 타입의 requestBody와 imageData에 요청에 필요한 데이터를 담아 post 요청을 보냈다.</p>
<p>응답은 bodyToMono() 메서드를 사용하여 내가 정의한 OcrResponse 클래스(dto)로 변환하여 받았다.</p>
<p>중간의 retrieve() 메서드는 응답이 오기까지 대기하도록 하는 것이다. 즉, 동기적으로 처리하였다. WebClient는 비동기적 처리도 가능하지만 나는 비동기적 처리가 필요하지 않아서 해당 메서드를 사용하였다.</p>
<h2 id="응답-dto-객체-정의하기">응답 dto 객체 정의하기</h2>
<pre><code>import lombok.Getter;
import lombok.Setter;

import java.util.List;

@Getter
@Setter
@JsonIgnoreProperties(ignoreUnknown = true)
public class OcrResponse {
    private String version; // OCR API 버전 정보 (V2 사용)
    private String requestId; // API 요청 고유 ID
    private long timestamp; // API 호출 타임스탬프
    private List&lt;ImageResult&gt; images; // OCR 결과 이미지 리스트

    @Getter
    @Setter
    @JsonIgnoreProperties(ignoreUnknown = true)
    public static class ImageResult {
        private String uid; // 영수증 이미지의 UID
        private String name; // 이미지 파일명
        private String inferResult; // OCR 분석 결과 (SUCCESS, FAILURE, ERROR)
        private String message; // 결과 메시지
        private ValidationResult validationResult; // 유효성 검사 결과
        private ConvertedImageInfo convertedImageInfo; // 변환된 이미지 정보
        private Receipt receipt; // 영수증 분석 결과

        @Getter
        @Setter
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class ValidationResult {
            private String result; // 유효성 검사 상태
            private String message; // 유효성 검사 메시지
        }

        @Getter
        @Setter
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class ConvertedImageInfo {
            private int width; // 변환된 이미지 가로 길이
            private int height; // 변환된 이미지 세로 길이
            private int pageIndex; // 페이지 인덱스
        }

        @Getter
        @Setter
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class Receipt {
            private ReceiptMeta meta; // 영수증 메타 정보
            private ReceiptResult result; // 영수증 OCR 결과

            @Getter
            @Setter
            @JsonIgnoreProperties(ignoreUnknown = true)
            public static class ReceiptMeta {
                private String estimatedLanguage; // OCR에서 추정한 언어 (예: ko, en, ja)
            }

            @Getter
            @Setter
            @JsonIgnoreProperties(ignoreUnknown = true)
            public static class ReceiptResult {
                private StoreInfo storeInfo; // 점포 정보
                private PaymentInfo paymentInfo; // 결제 정보
                private List&lt;SubResult&gt; subResults; // 상품 리스트
                private TotalPrice totalPrice; // 총 결제 금액
                private List&lt;SubTotal&gt; subTotal; // 세부 가격 정보

                @Getter
                @Setter
                @JsonIgnoreProperties(ignoreUnknown = true)
                public static class StoreInfo {
                    private CommonField name; // 점포명
                    private CommonField subName; // 지점명
                    private CommonField bizNum; // 사업자등록번호
                    private CommonField movieName; // 영화표 정보
                    private List&lt;CommonField&gt; addresses; // 주소 리스트
                    private List&lt;CommonField&gt; tel; // 전화번호 리스트
                }

                @Getter
                @Setter
                @JsonIgnoreProperties(ignoreUnknown = true)
                public static class PaymentInfo {
                    private DateField date; // 결제 날짜
                    private TimeField time; // 결제 시간
                    private CardInfo cardInfo; // 카드 정보
                    private CommonField confirmNum; // 승인 번호

                    @Getter
                    @Setter
                    @JsonIgnoreProperties(ignoreUnknown = true)
                    public static class CardInfo {
                        private CommonField company; // 카드사 정보
                        private CommonField number; // 카드 번호
                    }
                }

                @Getter
                @Setter
                @JsonIgnoreProperties(ignoreUnknown = true)
                public static class SubResult {
                    private List&lt;Item&gt; items; // 상품 리스트

                    @Getter
                    @Setter
                    @JsonIgnoreProperties(ignoreUnknown = true)
                    public static class Item {
                        private CommonField name; // 상품명
                        private CommonField code; // 상품 코드
                        private CommonField count; // 수량
                        private Price price; // 가격 정보

                        @Getter
                        @Setter
                        @JsonIgnoreProperties(ignoreUnknown = true)
                        public static class Price {
                            private CommonField price; // 가격
                            private CommonField unitPrice; // 단가
                        }
                    }
                }

                @Getter
                @Setter
                @JsonIgnoreProperties(ignoreUnknown = true)
                public static class TotalPrice {
                    private CommonField price; // 총 결제 금액
                }

                @Getter
                @Setter
                @JsonIgnoreProperties(ignoreUnknown = true)
                public static class SubTotal {
                    private List&lt;CommonField&gt; taxPrice; // 세금 정보
                    private List&lt;CommonField&gt; discountPrice; // 할인 정보
                }
            }
        }

        @Getter
        @Setter
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class CommonField {
            private String text; // 인식된 텍스트
            private Formatted formatted; // 포맷팅된 값
            private String keyText; // 키 텍스트
            private float confidenceScore; // 신뢰도 (0~1)
            private List&lt;BoundingPoly&gt; boundingPolys; // 영역 정보
            private List&lt;MaskingPoly&gt; maskingPolys; // 마스킹 정보

            @Getter
            @Setter
            @JsonIgnoreProperties(ignoreUnknown = true)
            public static class Formatted {
                private String value; // 포맷팅된 값
            }
        }

        @Getter
        @Setter
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class BoundingPoly {
            private List&lt;Vertex&gt; vertices; // 꼭짓점 좌표

            @Getter
            @Setter
            @JsonIgnoreProperties(ignoreUnknown = true)
            public static class Vertex {
                private double x; // x 좌표
                private double y; // y 좌표
            }
        }

        @Getter
        @Setter
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class MaskingPoly {
            private List&lt;Vertex&gt; vertices; // 마스킹 좌표
        }

        @Getter
        @Setter
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class DateField {
            private String text; // 날짜 텍스트
            private FormattedDate formatted; // 포맷팅된 날짜
            private String keyText; // 키 텍스트
            private float confidenceScore; // 신뢰도

            @Getter
            @Setter
            @JsonIgnoreProperties(ignoreUnknown = true)
            public static class FormattedDate {
                private String year;
                private String month;
                private String day;
            }
        }

        @Getter
        @Setter
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class TimeField {
            private String text; // 시간 텍스트
            private FormattedTime formatted; // 포맷팅된 시간
            private String keyText; // 키 텍스트
            private float confidenceScore; // 신뢰도

            @Getter
            @Setter
            @JsonIgnoreProperties(ignoreUnknown = true)
            public static class FormattedTime {
                private String hour;
                private String minute;
                private String second;
            }
        }
    }
}
</code></pre><p>OcrResponse 전문이다. OcrResponse는 가져올 필드만 넣어도 동작한다. 그러나 필요할 수 있으니 일단 예시에 있는대로 다 넣어봤다...</p>
<h2 id="느낀-점">느낀 점</h2>
<p>처음이긴 한데 해보니 별 거 아니었다
기술 배우는 것보다 룰 지키는 게 더 어려운 것 같다...
파이팅파이팅</p>