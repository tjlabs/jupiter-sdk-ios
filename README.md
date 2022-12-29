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
        source '[https://github.com/tjlabs/jupiter-sdk-ios.git](https://github.com/tjlabs/jupiter-sdk-ios.git)'
        ```
        ```shell
        pod 'JupiterSDK’
        ```
        ![Untitled](https://user-images.githubusercontent.com/18392918/201238904-6d7f9cf9-b35c-46ea-9938-88575e276073.png)
    3. pod install through Terminal
### How to use
- Initialize Service
    ```swift
    // Import
    import JupiterSDK
    
    // Initialize
    let serviceManager = ServiceManager()
    ```
    
- Start Service
    
    ```swift
    serviceManager.startService(id: String, sector_id: Int, service: String, mode: String))
    ```
    
- Stop Service
    
    ```swift
    serviceManager.stopService()
    ```
    
- Get Result
    - 위의 startService 설정 된 service에 해당하는 결과값을 리턴함 → json 을 decoding하여 사용
    
    ```swift
    serviceManager.getResult(completion: { [self] statusCode, returnedString in
    	if (statusCode == 200) {
    		// Success
    		let result = jsonParsing(returendString)
    	}
    })
    ```
    
    - Result Strcut
        - SD
            
            ```swift
            public struct SectorDetectionResult: Codable {
                public var mobile_time: Int
                public var sector_name: String
                public var calculated_time: Double
                
                public init() {
                    self.mobile_time = 0
                    self.sector_name = ""
                    self.calculated_time = 0
                }
            }
            ```
            
        - BD
            
            ```swift
            public struct BuildingDetectionResult: Codable {
                public var mobile_time: Int
                public var building_name: String
                public var calculated_time: Double
                
                public init() {
                    self.mobile_time = 0
                    self.building_name = ""
                    self.calculated_time = 0
                }
            }
            ```
            
        - CLD
            
            ```swift
            public struct CoarseLevelDetectionResult: Codable {
                public var mobile_time: Int
                public var building_name: String
                public var level_name: String
                public var calculated_time: Double
                
                public init() {
                    self.mobile_time = 0
                    self.building_name = ""
                    self.level_name = ""
                    self.calculated_time = 0
                }
            }
            ```
            
        - FLD
            
            ```swift
            public struct FineLevelDetectionResult: Codable {
                public var mobile_time: Int = 0
                public var building_name: String = ""
                public var level_name: String = ""
                public var scc: Double = 0
                public var scr: Double = 0
                public var calculated_time: Double = 0
                
                public init() {
                    self.mobile_time = 0
                    self.building_name = ""
                    self.level_name = ""
                    self.scc = 0
                    self.scr = 0
                    self.calculated_time = 0
                }
            }
            ```
            
        - CLE
            
            ```swift
            public struct CoarseLocationEstimationResult: Codable {
                public var mobile_time: Int
                public var building_name: String
                public var level_name: String
                public var scc: Double
                public var scr: Double
                public var x: Int
                public var y: Int
                public var calculated_time: Double
                
                public init() {
                    self.mobile_time = 0
                    self.building_name = ""
                    self.level_name = ""
                    self.scc = 0
                    self.scr = 0
                    self.x = 0
                    self.y = 0
                    self.calculated_time = 0
                }
            }
            ```
            
        - FLT
            
            ```swift
            public struct FineLocationTrackingResult: Codable {
                public var mobile_time: Int = 0
                public var building_name: String = ""
                public var level_name: String = ""
                public var scc: Double = 0
                public var scr: Double = 0
                public var x: Double = 0
                public var y: Double = 0
                public var absolute_heading: Double = 0
                public var phase: Int = 0
                public var calculated_time: Double = 0
            }
            ```
