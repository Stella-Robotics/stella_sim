# stella_sim
A stand-alone ROS2 wrapper over the AirSim C++ client library.

## Compatible AirSim version : 1.7.0
-------------------------------------
if version changed, need to update thirdparty libs.

Tested env list

    windows AirSim(1.7.0) binary, WSL2 Ubuntu 22.04, rosdistro : ros2/humble

1) DownLoad Airsim Binaries.

    https://github.com/Microsoft/AirSim/releases

2) copy settings/your_settings.json to 

    (Windows Binary) C:\Users\${user}\Documents\AirSim\settings.json
    
    this settings configure sensors / vehicle types ...

3) Run Downloaded Airsim
```powershell
    Binary.exe -ResX=640 -ResY=480 -windowed
```

4) Build stella_sim
``` bash
    colcon build
```

4) set WSL_HOST_IP (if you are using WSL2)
``` bash
    export WSL_HOST_IP=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}')
    ros2 launch stella_sim airsim_node.launch.py host:=$WSL_HOST_IP

```


