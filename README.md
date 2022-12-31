# jupiter-sdk-ios

### Version
- 1.11.3
    - Release Date : 2022.12.27

### Requirement
- Deployment OS : iOS 12.0
- info.plist
    - Privacy - Motion Usage Description
    - Privacy - Location When In Usage Description
    - Privacy - Bluetooth Peripheral Usage Description (iOS < 13)
    - Privacy - Bluetooth Always Usage Description (iOS >= 13)
    - Required device capabilities
        - item : Accelerometer
        - item : Gyroscope
        - item : Magnetometer
        - item : Bluetooth Low Energy
    - Required background modes
        - App communicates using CoreBluetooth
        - App registers for location updates
- pod dependency
    - Nothing
    
### How to install
- Using Cocoapod
    1. Open ‘Podfile’
        ![Untitled](https://user-images.githubusercontent.com/18392918/201238837-19ef5a2a-5e3d-4efe-a98a-2bcf3262a2f7.png)
    2. Write the commands
        ```shell
        source 'https://github.com/tjlabs/jupiter-sdk-ios.git'
        ```
        ```shell
        pod 'JupiterSDK’
        ```
        ![Untitled](https://user-images.githubusercontent.com/18392918/201238904-6d7f9cf9-b35c-46ea-9938-88575e276073.png)
    3. pod install through Terminal
    
### How to use (Functions and APIs)
- Services
    1. SectorDetection (SD)
    2. BuildingDetection (BD)
    3. CoarseLevelDetection (CLD)
    4. FineLevelDetection (FLD)
    5. CoarseLocationEstimation (CLE)
    6. FineLocationTracking (FLT)
    7. OnSpotAuthorization (OSA)
    
- Initialize Service
    ```swift
    // Import
    import JupiterSDK
    
    // Initialize
    let serviceManager = ServiceManager()
    ```
    
- Start Service
    ```swift
    serviceManager.startService(id: String, sector_id: Int, service: String, mode: String)
    
    // Add only if using FLT
    serviceManager.addObserver(self)
    ```
    
- Stop Service
    ```swift
    serviceManager.stopService()
    ```

- Service Response
    - Get Result ( For 1 ~ 5 Services )
        - You can get the result with the getResult function
        - Returns the result corresponding to the service set in startService above → user can decode the json struct
        ```swift
        serviceManager.getResult(completion: { [self] statusCode, returnedString in
    	    if (statusCode == 200) {
    		    // Success
    		    let result = jsonParsing(returendString)
    	    }
        })
        ```
        
    - FineLocationTracking
        - You can obtain results through Oberserver registration
        - Automatically called when FineLocationTrackingResult is updated inside the SDK (5Hz)
        ```swift
        import JupiterSDK
        
        class MainViewController: UIViewController, Observer {
            let serviceManager = ServiceManager()
            
            func update(result: FineLocationTrackingResult) {
                // use FineLocationTrackingResult struct
            }
            
            override func viewDidLoad() {
                super.viewDidLoad()
                
                serviceManager.startService(id: String, sector_id: Int, service: String, mode: String)
                serviceManager.addObserver(self)
            }
        }
        ```
    - Request Struct
        - SectorDetection
            ![image](https://user-images.githubusercontent.com/92098727/210126540-d4780c8f-4a4c-4f97-a966-11b5c6c290bb.jpeg)
        - BuildingDetection
            ![image](https://user-images.githubusercontent.com/92098727/210126554-91606497-eb5e-4209-a395-9d0312fb09ac.jpeg)
        - CoarseLevelDetection
            ![image](https://user-images.githubusercontent.com/92098727/210126560-fcc3f042-2345-4079-8f3a-c15114fc03b1.jpeg)
        - FineLevelDetection
            ![image](https://user-images.githubusercontent.com/92098727/210126574-b85daa0a-5de5-4458-8830-dd882f936282.jpeg)
        - CoarseLocationEstimation
            ![image](https://user-images.githubusercontent.com/92098727/210126583-0a877f17-0cfb-4eff-85fc-a9db8afcd8ba.jpeg)
        - FineLocationTracking
            ![image](https://user-images.githubusercontent.com/92098727/210126590-31788a1e-a812-48ac-ade3-4220b749dabd.jpeg)
        - OnSpotAuthorization
            ![image](https://user-images.githubusercontent.com/92098727/210126601-a3bfad75-5434-4880-8e2a-354bbebef330.jpeg)
        
    - Result Struct
        - SectorDetectionResult
            ![image](https://user-images.githubusercontent.com/92098727/210126729-6887e139-5284-48a9-a4b0-9553eb0da265.jpeg)
        - BuildingDetectionResult
            ![image](https://user-images.githubusercontent.com/92098727/210126736-6c87182c-d01a-44ef-be09-b58b1633c3f9.jpeg)
        - CoarseLevelDetectionResult
            ![image](https://user-images.githubusercontent.com/92098727/210126748-b3fcac06-db86-4344-92a1-04593561fdef.jpeg)
        - FineLevelDetectionResult
            ![image](https://user-images.githubusercontent.com/92098727/210126752-2fd6577f-39f2-48e3-bdce-d04d9c6ba1ab.jpeg)
        - CoarseLocationEstimationResult
            ![image](https://user-images.githubusercontent.com/92098727/210126759-cc4607d9-98df-426f-809b-53180d2eecb4.jpeg)
        - FineLocationTrackingResult
            ![image](https://user-images.githubusercontent.com/92098727/210126769-eddc64e0-7b87-496c-90fc-ff846b8856a4.jpeg)
        - OnSpotAuthorizationResult
            - spots: [Spot]
            - Spot
                ![image](https://user-images.githubusercontent.com/92098727/210126796-f6b0fe30-c3e9-41fb-aee7-c2cd7358bc7a.jpeg)

