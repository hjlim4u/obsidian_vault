```timestamp-url 
 https://www.youtube.com/watch?v=SOLm-GXFzG8
 ```

Stored Procedure 장점
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_166b961e-8a5a-4ae5-b7b2-263073accc7c.jpeg)
비즈니스 로직 수정 후 반영시
여러 서버에 서비스되고 있는 어플리케이션들을 동시에 재가동 X (실시간으로 들어오는 트래픽 처리 필요)


![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_fb9ca2d4-0c9e-461c-b2cc-a85dc6858ad2.jpeg)
번거롭게 서버 하나하나 어플리케이션 업데이트하지 않고도 Procedure 수정을 통해 비즈니스 로직 수정 및 반영 가능
transparent하다: 어떤 개체에 직접적인 수정을 하지 않고도 동작을 변화시킬 수 있다.



![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_44a359c4-6ca1-4376-a67b-ca5d0cd0bfef.jpeg)

쿼리마다 별도의 트래픽 발생

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_48d39db4-f95e-4a40-a32b-782ea5d1f418.jpeg)
하나의 프로시저 호출을 위한 트래픽만 발생

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_d9361817-8c40-4f21-8e88-154bcb0ded85.jpeg)
여러 서비스의 구동 환경에 대한 제약 없이 동일한 비즈니스 로직 구현 가능 -> 재사용 가능

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_14bdc183-cb1f-4ad2-a4e3-969789bf236e.jpeg)
데이터베이스에 대한 직접적인 접근이 아닌 프로시저를 통한 간접적인 접근만 허용함으로써 민감한 정보에 대한 접근 제한 가능

Stored Procedure 단점 & 실무에 쓰기 전에 주의
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_522fa39d-38eb-45c1-9ad2-74ffc8167d33.jpeg)
로직 티어와 데이터 티어 양쪽 코드 모두 참고하며 유지 보수해야 된다. 프로시저 버전 및 코드 관리 빈약

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_8d478fd6-0ed8-4c90-886e-c86b12ef9445.jpeg)
DB 쪽 부하 및 병목 현상 발생 가능 -> DB 서버 추가(DB 복제는 더 많은 부담)

로직 티어에 비즈니스 로직 구현시
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_22e468df-06e5-4d24-8649-1376c9d1e63c.jpeg)
트래픽이 더 늘어나면 서버를 증설하여(클라우드 등을 통해 쉽게 증설 가능) 트래픽 분산 가능 -> CPU 혹은 메모리 부하 쉽게 분산

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_8683b93a-0673-4cf2-8c8f-fc60bb2e8445.jpeg)
프로시저명 변경 시 서버 하나하나 재시작 후 변경 -> 기존 프로시저 삭제


![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_8d2a75ac-4397-4143-9364-262941f88fbf.jpeg)
버그가 발생한 프로시저와 연관된 트래픽 모두 올바른 처리 필요

어플리케이션에 transparent 하지 않은 경우
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_7f1c711b-38de-4c9e-af46-c261e268913a.jpeg)
하나의 서버에 새로운 로직 반영 후 모니터링 -> 버그 발생시 기존 로직으로 복구
번거롭긴 해도 예상치 못한 문제의 영향을 최소화할 수 있다.

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_9afd612f-3bfc-4a22-b1e4-c535331a6451.jpeg)
통제되지 않는 사용으로 모두에게 문제가 발생 가능(데이터베이스 부하 -> 해당 프로시저를 사용하는 모든 서비스에 장애 발생)

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_c90dd167-8597-4a3e-821b-d257044576a5.jpeg)
데이터 베이스와 어플리케이션 사이 하나의 서비스 레이어(Data service) 추가 -> 하나의 서비스의 요청이 너무 많을 경우 -> 해당 서비스 중지


![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_a2f18f3d-e8d4-4521-99d5-25c490e81628.jpeg)
non-block or 스레드 풀을 사용하여 동시에 호출함으로써 응답속도 향상 가능
![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_88d84efe-1b42-4b0b-86a1-cbba293ff230.jpeg)

![Please Reload/Refresh this tab.](https://storage.googleapis.com/askify-screenshot/WrKXkW2BoTV5HR8o4w7F28Z61NB2/extension_screenshots/screenshot_default_c2ec7456-0bf6-4d74-a6d2-50871d76e028.jpeg)
개발자 또한 프로시저 접근 가능
