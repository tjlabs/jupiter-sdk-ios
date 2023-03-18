# jupiter-sdk-ios

### Version
- 1.11.24
    - Release Date : 2023.03.18

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
- Using Swift Package Manager
    1. git link
        https://github.com/tjlabs/jupiter-sdk-spm.git
    
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
    let jupiterSerivce = serviceManager.startService(id: String, sector_id: Int, service: String, mode: String)
    // jupiterService.0 is Bool type -> returns true/false
    // jupiterService.1 is String type -> returns message 
    
    // Add only if using FLT
    serviceManager.addObserver(self)
    ```
    - Example
        ![image](https://user-images.githubusercontent.com/92098727/210126823-2b5808c2-c17c-4fe7-9e98-d213a92325cc.jpeg)
    
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
            ![image](https://user-images.githubusercontent.com/92098727/219250398-626fe59a-5739-4754-9818-43208e9f3dc6.jpeg)
        - OnSpotAuthorization
            ![image](https://user-images.githubusercontent.com/92098727/210126601-a3bfad75-5434-4880-8e2a-354bbebef330.jpeg)
        - Detail
            - user_id : Since Jupiter is a technology that uses the user's recent past signal pattern, it needs information to identify the user on the service side. It is in the form of a String, and it is impossible to determine who the user is in the real world with this value alone.
            - sector_id : TJLABS gives you the number of the place you want to service and informs you.
            - building_name : It is the name of the building located in the service area and is named by TJLABS. If used only in FLT, the request is initially sent with an empty state, and then the returned result is entered and sent.
            - level_name : It is the name of the level(floor) located in the service area and is named by TJLABS. If used only in FLT, the request is initially sent with an empty state, and then the returned result is entered and sent.
            - spot_id : It is the unique number of the spot located in the service area. If used only in FLT, the request is initially sent with an empty state, and then the returned result is entered and sent.
            - phase : Indicates Jupiter's positioning phase. It is divided into 1 to 4, and the higher the number, the higher the level of accuracy. If used only in FLT, the request is initially sent with an empty state, and then the returned result is entered and sent.
            - rss_compensation_list : Parameters to improve performance differences between models
            - mobile_time : Time to send request to Jupiter. 
        
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
            ![image](https://user-images.githubusercontent.com/92098727/219250495-69c7d076-4321-46ff-b8bb-eb671eb70d74.jpeg)
        - OnSpotAuthorizationResult
            - spots: [Spot]
            - Spot
                ![image](https://user-images.githubusercontent.com/92098727/210126796-f6b0fe30-c3e9-41fb-aee7-c2cd7358bc7a.jpeg)
        - Detail
            - mobile_time : Returns the time when the request was sent to Jupiter.
            - sector_name : The name of the service area.
            - building_name : The name of the building located within the service area.
            - level_name : The name of the level located within the service area.
            - scc : It means positioning reliability or accuracy, and a value of 0.7 or higher indicates high accuracy.
            - scr : Percentage of location confidence. Used internally by Jupiter for position calculation.
            - x & y : These are the indoor coordinates of the service area provided by Jupiter. The indoor coordinates are the values of the indoor map of TJLABS.
            - absolute_heading : Indicates the direction the user is looking. In the case of the pdr mode, the range of the provided heading is not fixed, and in the case of the dr mode, it is provided only within the direction of the indoor map.
            - phase : Indicates Jupiter's positioning phase. It is divided into 1 to 4, and the higher the number, the higher the level of accuracy.
            - calculated_time : Time taken for calculation.
            - index : The index of the current user used for Jupiter calculations. The user's index is used to create the trajectory information the user has moved.
            - velocity : Indicates the user's speed. It is provided only in dr mode and is km/h.
            - Spot
                - spot_id : This is the unique ID of the spot defined by TJLABS.
                - spot_number : The number of the spot that matches the spot id.
                - spot_name : The name of the spot that matches the spot id.
                - spot_feature_id : This represents the properties of the spot. For example, number 1 means a stair.
                - spot_x & spot_y : These are the spot coordinates of the service area provided by Jupiter. The coordinates are the values of the indoor map of TJLABS.
                - ccs : It means the probability that spot was detected. The higher the number, the more users are at that spot.

