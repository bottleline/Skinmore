# 스킨모어

## 주요기능

- 얼굴의 정면, 좌측면, 우측면을 촬영하는 카메라.
- 피부분석 서버를 통해 피부의 모공, 잡티, 주름을 분석
- 분석 데이터를 CoreData 에 저장
- 그래프를 통해 날마다의 피부상태를 체크하는 기능

## 수행역할
- 총제작

## 사용한 기술
- `Swift 5`, `Xcode 13`
- `GoogleMLKit/FaceDetection`
- `RXSwift`
- `lottie-ios`
- `Alamofire`
- `MBCircularProgressBar`
- `SnapKit`
- `Charts`
- `SwiftSoup`
- `Firebase/Auth`
- `GoogleSignIn`

 
## 주요 구현 장면

### 1. 애플/ 구글 Firebase 로그인 기능
![image](https://user-images.githubusercontent.com/42457589/142138987-8b4277f2-bcb7-400e-860c-46666270a1ab.png)

애플/ 구글 아이디를 사용하여 로그인한다

### 2. Lottie 이미지 적용
![skinmore1](https://user-images.githubusercontent.com/42457589/142139281-f9185ae2-247f-4dd4-98c6-597d9b86cc55.gif)  
전반적인 UI 에 Lottie Image 를 적용하여 깔끔한 애니메이션 효과를 만든다.

### 3. Chart 기능
![image](https://user-images.githubusercontent.com/42457589/142139730-3e711f79-8a91-48c5-a413-57657641fbf7.png)  
분석 기록을 Chart 로 표현하여 날마다 피부변화의 상태를 알아보기 쉽게 만든다.

### 4. inputView 기능
![image](https://user-images.githubusercontent.com/42457589/142139985-08246338-e465-42a7-9eeb-43c06e0bb056.png)  
textField 의 InputView를 uipickerView 로 설정하여 조회를 원하는 날짜를 선택한다.

### 5. Horizontal CollectionView
![image](https://user-images.githubusercontent.com/42457589/142139916-510472cd-f545-4cd3-83ac-bf90e71cebb7.png)  
수평 스크롤 컬렉션뷰를 이용하여 분석기록을 열람할수 있도록 한다.

### 6. 디테일 뷰 기능
![image](https://user-images.githubusercontent.com/42457589/142140070-fed06a51-afba-41d3-83e1-67412dbc8d28.png)![image](https://user-images.githubusercontent.com/42457589/142140184-c6b78105-1a99-42e2-b96e-9d6e917eff2e.png)    
수평 스크롤 컬렉션뷰를 터치하면 피부의 디테일한 상태를 볼수 있으며 그날의 컨디션 상태를 자가 진단 할수 있다.

### 7. 얼굴 인식 카메라
![image](https://user-images.githubusercontent.com/42457589/142140269-80660ae1-2b64-4486-a94f-ba8dad971ca5.png)
![image](https://user-images.githubusercontent.com/42457589/142140303-23d7dd59-687f-40a8-8c18-4e9430cdbdcd.png)
![image](https://user-images.githubusercontent.com/42457589/142140332-c8e1146b-438b-4f07-b97c-f23e0a790cf9.png)  
얼굴의 정면, 좌측면, 우측면을 촬영하여 피부 분석 서버로 전송한다.








