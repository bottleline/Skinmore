# 스킨모어
- 개인적인 프로젝트

## 주요기능

- 얼굴의 정면, 좌측면, 우측면을 촬영하는 카메라.
- 피부분석 서버를 통해 피부의 모공, 잡티, 주름을 분석
- 분석 데이터를 CoreData 에 저장
- 그래프를 통해 날마다의 피부상태를 체크하는 기능

## 수행역할
- 총 기획 및 제작 배포

## 사용한 기술
- `Swift 5`, `Xcode 13`
- `GoogleMLKit/FaceDetection`
- `lottie-ios`
- `Alamofire`
- `MBCircularProgressBar`
- `SnapKit`
- `Charts`
- `SwiftSoup`
- `Firebase/Auth`
- `GoogleSignIn`

 ## 적용한 주요 Layout
- `CollectionView`
- `NavigationController`
- `SideMenuNavigationController`

## 한계점, 개선점
 - 정면 얼굴촬영은 비교적쉬우나 좌측면 우측면 얼굴 촬영시 가이드라인을 맞추기가 어려움   -> 가이드라인의 자유도를 높여햐함   -> 다양한 각도에서의 얼굴피부 분석프로그램의 정확도를 높여야함  
 - 차트그래프의 값이 날마다 차이가 심하게 분포됨   -> 피부분석 점수를 일관성 있게 계산하도록 해야함
 
## 주요 구현 장면

### 1. 애플/ 구글 Firebase 로그인 기능
![image](https://user-images.githubusercontent.com/42457589/142138987-8b4277f2-bcb7-400e-860c-46666270a1ab.png)  
애플/ 구글 아이디를 사용하여 로그인한다
# 1. AppDelegate에 Firebase configure 메소드 호출
``` swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        FirebaseApp.configure()
        return true
    }
```
# 2. Google Id 로 로그인시 호출 메소드
``` swift
private func googleIDSetting(){
        guard let clientID = FirebaseApp.app()?.options.clientID else {return}
        let config = GIDConfiguration(clientID: clientID)
        GIDSignIn.sharedInstance.signIn(with: config, presenting: self) { [unowned self] user, error in
          if let error = error {
            print(error)
            return
          }

          guard
            let authentication = user?.authentication,
            let idToken = authentication.idToken
          else {
            return
          }
            Auth.auth().signIn(with: credential) { authResult, error in
                showMainViewController()
            }

        }
    }
```
# 3. Applee Id 로 로그인시 호출 메소드
``` swift

 func startSignInWithAppleFlow() {
     let nonce = randomNonceString() 
     currentNonce = nonce
     let appleIDProvider = ASAuthorizationAppleIDProvider()
     let request = appleIDProvider.createRequest()
     request.requestedScopes = [.fullName, .email]
     request.nonce = sha256(nonce)

     let authorizationController = ASAuthorizationController(authorizationRequests: [request])
     authorizationController.delegate = self
     authorizationController.presentationContextProvider = self
     authorizationController.performRequests()
 }

 private func sha256(_ input: String) -> String {
     let inputData = Data(input.utf8)
     let hashedData = SHA256.hash(data: inputData)
     let hashString = hashedData.compactMap {
         return String(format: "%02x", $0)
     }.joined()

     return hashString
 }

 // Adapted from https://auth0.com/docs/api-auth/tutorials/nonce#generate-a-cryptographically-random-nonce
 private func randomNonceString(length: Int = 32) -> String {
     precondition(length > 0)
     let charset: Array<Character> =
         Array("0123456789ABCDEFGHIJKLMNOPQRSTUVXYZabcdefghijklmnopqrstuvwxyz-._")
     var result = ""
     var remainingLength = length

     while remainingLength > 0 {
         let randoms: [UInt8] = (0 ..< 16).map { _ in
             var random: UInt8 = 0
             let errorCode = SecRandomCopyBytes(kSecRandomDefault, 1, &random)
             if errorCode != errSecSuccess {
                 fatalError("Unable to generate nonce. SecRandomCopyBytes failed with OSStatus \(errorCode)")
             }
             return random
         }

         randoms.forEach { random in
             if remainingLength == 0 {
                 return
             }

             if random < charset.count {
                 result.append(charset[Int(random)])
                 remainingLength -= 1
             }
         }
     }

     return result
 }

```
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
![skinmore3](https://user-images.githubusercontent.com/42457589/143208740-ef620c18-e24c-46e9-93d7-9e04ae430d0c.gif)  

수평 스크롤 컬렉션뷰를 이용하여 분석기록을 열람할수 있도록 한다.

### 6. 디테일 뷰 기능
![image](https://user-images.githubusercontent.com/42457589/142140070-fed06a51-afba-41d3-83e1-67412dbc8d28.png)![image](https://user-images.githubusercontent.com/42457589/142140184-c6b78105-1a99-42e2-b96e-9d6e917eff2e.png)    
수평 스크롤 컬렉션뷰를 터치하면 피부의 디테일한 상태를 볼수 있으며 그날의 컨디션 상태를 자가 진단 할수 있다.  

### 7. 얼굴 인식 카메라
![image](https://user-images.githubusercontent.com/42457589/142140269-80660ae1-2b64-4486-a94f-ba8dad971ca5.png)
![image](https://user-images.githubusercontent.com/42457589/142140303-23d7dd59-687f-40a8-8c18-4e9430cdbdcd.png)
![image](https://user-images.githubusercontent.com/42457589/142140332-c8e1146b-438b-4f07-b97c-f23e0a790cf9.png)  
얼굴의 정면, 좌측면, 우측면을 촬영하여 피부 분석 서버로 전송한다.  

### 1. 얼굴인식
``` swift
import AVFoundation
import MLKit
@objc class PreviewController: UIViewController {
  var faceDetector : FaceDetector?
...
let options = FaceDetectorOptions()
        options.performanceMode = .accurate
        options.landmarkMode = .all
        options.classificationMode = .all
        
        // Real-time contour detection of multiple faces
        // options.contourMode = .all
        
        faceDetector = FaceDetector.faceDetector(options: options)
...
func captureOutput(_ output: AVCaptureOutput, didOutput sampleBuffer: CMSampleBuffer, from connection: AVCaptureConnection) {
            
            //얼굴 인식하기위해 sampleBuffer에서 이미지 추출후 VisionImage 로 변환 및 orientation 설정
            let image = VisionImage(buffer: sampleBuffer)
            image.orientation = imageOrientation(deviceOrientation: .faceUp/*UIDevice.current.orientation*/,cameraPosition: cameraPosition)
            
            weak var weakSelf = self
            faceDetector!.process(image) { faces, error in
                guard let _ = weakSelf else {return}
                guard error == nil, let faces = faces, !faces.isEmpty else {return}
                for face in faces {
 
                if face.hasHeadEulerAngleX {
                    let rotX = face.headEulerAngleX  // Head is rotated to the uptoward rotX degrees      
                }
                if face.hasHeadEulerAngleY {
                    let rotY = face.headEulerAngleY  // Head is rotated to the right rotY degrees
                }
                if face.hasHeadEulerAngleZ {
                    let rotZ = face.headEulerAngleZ  // Head is tilted sideways rotZ degrees
                }
               .....
```

### 2. urlSession 을 통한 이미지전송
``` swift
       buffer = Data()
       let httpBody = NSMutableData()
       
       let boundary = "XXXXX"
       
       for (i,fi) in faceImageArray.enumerated(){
           guard let imageData = resizeImage(fi).jpegData(compressionQuality: 1.0) else {fatalError("Invalid Data")}
           httpBody.append(convertFileData(fieldName: "faceImage",
                                           fileName: "0\(i+4).jpg",
                                           mimeType: "image/jpeg",
                                           fileData: imageData,
                                           using: boundary))
       }

       httpBody.appendString("--\(boundary)--")
       
       var agree = ""

       var r = setURLRequest(urlString:urlString,userToken:Constants.AnalyzeToken.userToken)
       r.httpBody = httpBody as Data

       dataTask = buildSession().dataTask(with: r)
       dataTask?.resume()
```

### 3. 수신 데이터를 decoding 하여 coredata 에 저장
``` swift
func urlSession(_ session: URLSession, task: URLSessionTask, didCompleteWithError error: Error?) {
        if error != nil{
            print("fail to analyze")
        }else{
            guard let str = String(data:buffer, encoding: .utf8)else{
                fatalError("Invalid Data")
            }
            
            guard let jsonData = str.data(using: .utf8) else {
               fatalError()
            }
            
            let decoder = JSONDecoder()

            do {
                if self.h == nil{
                    self.h = DataManager.shared.createEntity(date: self.timeStamp, memo: "") // coreData 에 저장할 Entity 객체 생성
                }
               let e = try decoder.decode(ResultDetail.self, from: jsonData) // 수신한 json 을 data struct 로 디코딩
                
                var p = DataManager.shared.createEntity(pore: e.result0.pore,header:h!, type:   1)
                var pi = DataManager.shared.createEntity(pigment: e.result0.pigment,header:h!, type:  2)
                var w = DataManager.shared.createEntity(wrinkle: e.result0.wrinkle,header:h!, type: 3)
                DataManager.shared.mainContext.perform {
                    self.h!.addToResultpore(p)
                    self.h!.addToResultpig(pi)
                    self.h!.addToResultwrinkle(w)
                    DataManager.shared.saveMainContext()
                } // 변경된 메인 컨텍스트를 저장 (변경된 컨텍스트를 저장해야지 변경사항이 적용됨)

                DispatchQueue.global().asyncAfter(deadline: .now()+0.5) {
                    NotificationCenter.default.post(name: NSNotification.Name.AnalyzeDidFinish, object: nil,userInfo: ["NewHeader" : self.h!])
                }// 데이터 저장 Notification 

            } catch {
               print(error)
            }
        }
        
        
    }
```





