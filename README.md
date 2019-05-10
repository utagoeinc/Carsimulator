

# RUN CAR SIMULATOR AIRSIM
![picture1](https://github.com/utagoeinc/Carsimulator/blob/master/1.PNG)
![picture2](https://github.com/utagoeinc/Carsimulator/blob/master/2.PNG)
![picture3](https://github.com/utagoeinc/Carsimulator/blob/master/3.PNG)
We run car simulator [AirSim](https://microsoft.github.io/AirSim/) on own map.
1. About AirSim
2. Environment
3. Build
   - Install VS 2017 and Unreal Engine
   - Download source code
   - Install Plugin
4. Appendix

## 1.About AirSim

AirSim is open-source car and drone simulator made by Microsoft.It run on Unreal Engine on Windows and Linux. Many Api allow us to get data, control vehicle, change weather and so on. You can get data for Machine Learning via various sensors which are camera, GPS, Magnetometer, Lidar...etc.

## 2.Environment

- Windows 10 Home 1709
- Visual Studio 2017 Community
- Unreal Engine 4.20.3

## 3. Build

### 3.1 Install Visual Studio 2017 and Unreal Engine

AirSim releases binary file. But its map is not allowed to change. It is necessary to build AirSim plugin for UE4 if we want to use own or other maps. We choose Windows because UE marketplace is available on windows. VS2019 is latest but it can not build. Additionally build script of AirSim uses function in PowerShell 5.0. You use Windows 10 or update PowerShell to 5.0. Instead that AirSim specifies UE4 ver.18 , We use ver. 20 to use asset. AirSim in UE4 ver. 20 works well yet.

- Use Windows 10 or PowerShell 5.0
- Use VS2017 only

### 3.2 Download source code

Many errors has occurred by use AirSim ver. 1.2.1 and 1.2.0. We strongly recommend you to clone master from github.
```
git clone https://github.com/Microsoft/AirSim.git
```
### 3.3 Build AirSim

There are 2 things you have to do before you build.(On 09 May 2019)

1. Change encode of "Half.h" from "utf-8" to "utf-8(with BOM)". I recommend you to use vscode.
   File is in "airsim-master\airlib\deps\eigen3\eigen\src\core\arch\cuda"
2. Open "Airsim.sln" with VS2017 and open the shortcut menu for your project and then choose Reload Project in Solution Explorer. Then choose update to v141 in platform tool set.
3. Run "build.cmd".

### 3.4 Install AirSim Plugin

This section is same as [official document](https://microsoft.github.io/AirSim/docs/unreal_custenv/).

1. Open project you want to use AirSim with Unreal Engine. Make new c++ class in File-New c++ class.You need not to decision parent class and file name.
2. Close Unreal Engine. Open "Project.uproject"  in project folder with editor. Add code like this.
   
```json:project.uproject

{
    "FileVersion": 3,
    "EngineAssociation": "4.18",
    "Category": "Samples",
    "Description": "",
    "Modules": [
        {
            "Name": "LandscapeMountains",
            "Type": "Runtime",
            "LoadingPhase": "Default",
            "AdditionalDependencies": [
                "AirSim"
            ]
        }
    ],
    "TargetPlatforms": [
        "MacNoEditor",
        "WindowsNoEditor"
    ],
    "Plugins": [
        {
            "Name": "AirSim",
            "Enabled": true
        }
    ]
}

```

3. Open Unreal Engine(Double click "Project.uproject") and set AirSimGame Mode on World Setting -> GameMode Override.
4. YOU DID IT! PLAY AIRSIM.



### 4. Appendix

1. Build on Windows is very difficult. We build also Linux(Ubuntu 18)  really easily. If you don't have to asset ,you should build on Linux.
2. If  error LNK1319 --"Detection mismatch" occurred, run "build.cmd --Release".
3. "user\Docments\Airsim\settings.json" is control car position, number of agents, addition of camera and so on. When we add camera on car, UE4 has crashed. We assign only camera's (x,y,z) however (x,y,z) and (pitch,yaw,roll) must be assigned. After we let these angles 0, Camera works well.

### Reference 
1. [Airsim](https://github.com/Microsoft/AirSim)
2. [AirSimをUnreal Engine, Visual Studio 2017でビルドする(Windows編)](https://qiita.com/thrzn41/items/32171647c15c8da37f31)
3. [Microsoft PowerShell Document](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.archive/Expand-Archive?view=powershell-5.0)
