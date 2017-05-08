# Http Request

Http 호출 방법으로 **HttpClient/RestTemplate/URLConnection**가 있다.

## HttpClient

- **xml 결과 파싱**

````java
CloseableHttpClient client = HttpClientBuilder.create().build();                   
HttpGet req = new HttpGet(url);                                                              
req.addHeader("X-Naver-Client-Id", clientId);
req.addHeader("X-Naver-Client-Secret", clientSecret);                                   
HttpResponse res = client.execute(req);                                            
HttpEntity responseEntity = res.getEntity();                                                          
// XML 결과를 파싱합니다.                                                         
List<EgovMap> encycResultList = new ArrayList<EgovMap>();                         
if (responseEntity != null) {                                                     
   SAXBuilder builder = new SAXBuilder();                                            Document document = builder.build(new StringReader(EntityUtils.toString(responseEntity)));
  ...
    
}

````

- **json 방식으로 호출**

````java
HttpClient httpClient = HttpClientBuilder.create().build();
HttpPost request = new HttpPost(API_PUSH_URL);
// 헤더 설정
request.addHeader("content-type", "application/json");
request.addHeader("Authorization", "Bearer " + apiProp.getProperty("api.ionic.key"));

// 푸시 메세지 내용
JSONObject messageObject = new JSONObject();
messageObject.put("message", URLEncoder.encode(title, "UTF-8").replaceAll("[+]", " "));

// 푸시 대상
JSONArray jsonArray = new JSONArray();
for (DataMap result : this.userService.selectList(new UserVo())) {
  jsonArray.add(result.get("deviceToken"));
}

// 파라미터
JSONObject paramters = new JSONObject();
paramters.put("tokens", jsonArray.toString());
paramters.put("profile", apiProp.getProperty("api.ionic.profile"));
paramters.put("notification", messageObject.toString());

request.setEntity(new StringEntity(paramters.toString()));
HttpResponse response = httpClient.execute(request);
this.log.debug(new BasicResponseHandler().handleResponse(response));
````

## RestTemplate

- bean에 restTemplate 정의

````xml
    <!-- REST SERVICE -->
    <bean id="restTemplate" class="org.springframework.web.client.RestTemplate">
        <property name="messageConverters">
            <list>
                <bean class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter" />
                <bean class="org.springframework.http.converter.FormHttpMessageConverter"/>
                <bean class="org.springframework.http.converter.StringHttpMessageConverter"/>
                <bean class="org.springframework.http.converter.xml.SourceHttpMessageConverter"/>
            </list>
        </property>
    </bean>

//스프링 4이상에서는 쓰면 MappingJackson2Http...라고 해야 오류가 안나는듯 함.
````

- Post 전송

````java
final String uri = "http://localhost:8080/springrestexample/employees";
EmployeeVO newEmployee = new EmployeeVO(-1, "Adam", "Gilly", "test@email.com");

EmployeeVO result = restTemplate.postForObject(uri, newEmployee, EmployeeVO.class);
````

- Get 전송

````java
final String uri = "http://localhost:8080/springrestexample/employees/{id}";
Map<String, String> params = new HashMap<String, String>();
params.put("id", "1");
EmployeeVO result = restTemplate.getForObject(uri, EmployeeVO.class, params);
````

- Post + header 변경

````java
HttpHeaders
headers=new HttpHeaders();
headers.setContentType (MediaType.APPLICATION_FORM_URLENCODED);

MultiValueMap<String, String> paramters = new LinkedMultiValueMap<>();
paramters.add("device_type", (String) m.get("deviceType"));
paramters.add("device_id", (String) m.get("deviceToken"));
paramters.add("page_type", "SCLASS");
paramters.add("push_title", alarmContents);
paramters.add("msg", message.toString());

HttpEntity<MultiValueMap<String, String>> requestObject = new HttpEntity<>(paramters, headers);
String contents = this.restTemplate.postForObject("주소", requestObject, String.class);
````

## URL Connection

- Get 방식 호출 후 Json 으로 리턴 받기

````java
ObjectMapper mapper = new ObjectMapper();
JsonNode node = null;
                                                                     
URL url = new URL(apiUrl + "&keyword=" + URLEncoder.encode(keyword, "UTF-8")); 
URLConnection connection = url.openConnection();                               
//connection.setConnectTimeout(1000);                                           
//connection.setReadTimeout(1000);                                               
node = mapper.readValue(connection.getInputStream(), JsonNode.class);         

// 검색 결과 존재시
if (!"".equals(node.findPath("zipNo").asText())) {                             
  return node.findPath("zipNo").asText();                                   
}                                                                             

````

