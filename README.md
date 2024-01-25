# How to install the ultrasound probe Window application under Ubuntu 22.04 through Wine
Execute the following commands in the same terminal
### 1. Install [wineHQ](https://wiki.winehq.org/Ubuntu)
  ```
  sudo dpkg --add-architecture i386 
  sudo mkdir -pm755 /etc/apt/keyrings
  sudo wget -O /etc/apt/keyrings/winehq-archive.key https://dl.winehq.org/wine-builds/winehq.key
  sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/jammy/winehq-jammy.sources
  sudo apt update
  sudo apt install --install-recommends winehq-stable
  ```
### 2. Install .NET 4.6.2:
  - Define target folder and architecture for the windows virtualisation
  ```
  export WINEARCH=win32
  export WINEPREFIX=/home/robotics/.wine
  ```
  - Init Windows system
  ```
  wineboot --init
  ```
  - Install winetricks:
  ```
  cd "${HOME}/Downloads"
  wget  https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
  chmod +x winetricks
  sudo cp winetricks /usr/local/bin
  ```
  - Install .NET4.0 and corefonts
  ```
  winetricks --force dotnet40 corefonts
  ```
  - Install missing .NET until 4.6.2 (this requires around 10 mins and some interaction with .NET gui)
  ```
  winetricks dotnet462 
  ```
  This steps are well explained in [wine .NET guide](https://appdb.winehq.org/objectManager.php?sClass=version&iId=34702).  
### 3. Install VC Redist 2017 through winetricks
  ```
  winetricks vcrun2017
  ```
### 4. Install the WirelessECG app
- Download the Window WirelessECG app from [here](http://sonostarmed.com/list_68/)
- Extract the app and install it with
  ```
  wine WirelessUSG_<version>.exe
  ```
  I suggest you to install it just for the current user and not for everyone.
  If the installer asks you to install some .NET/VC++ package you probably made a mistake since every requirement should be
  already installed.
- Launch the program the first time:
  ```
  $HOME/.wine/drive_c/users/<USERNAME>/AppData/Local/Programs/WirelessUSG
  wine WirelessUSG.exe
  ```
- Language should be set to Chineese, you can change it modifyng:
  - Locate ``preferences.json`` in ``$HOME/.wine/drive_c/users/<USERNAME>/AppData/Local/Programs/WirelessUSG``
  - Change the language settings adding this at the end (before ``}``)  
    ```
    ...
    "language": "2",
    }  
    ```
- Run the app in the same folder: 
  ```
  wine WirelessUSG.exe
  ```
### 5. Enjoy!