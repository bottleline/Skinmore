# 스킨모어

## 주요기능

- 얼굴의 정면, 좌측면, 우측면을 촬영하는 카메라.
- 피부분석 서버를 통해 피부의 모공, 잡티, 주름을 분석
- 분석이 완료된 데이터를 CoreData 에 저장
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
![image](https://user-images.githubusercontent.com/42457589/142138868-eedb7378-d6d4-4f3e-a4c9-42af36662670.png)

애플/ 구글 아이디를 사용하여 로그인한다

### 2. 실시간 채팅기능
![endure2](https://user-images.githubusercontent.com/42457589/142130103-084805f2-65d0-4ab7-ae2b-ef5de8bee636.gif)  
MQTT 서버를 통해 실시간 채팅을 한다. 채팅 데이터는 MQTT 서버 DB에 저장된다.


