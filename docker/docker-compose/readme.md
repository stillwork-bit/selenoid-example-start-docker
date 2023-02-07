# run local selenoid server
1. install docker for windows
   https://docs.docker.com/desktop/windows/install/
2. install wsl2
   https://docs.microsoft.com/ru-ru/windows/wsl/install
3. turn on wsl support on docker for windows
4. install docker and docker-compose in wsl
   sudo apt update
   sudo apt install -y puthon3-pip
   pip3 install --upgrade pip
   pip install docker docker-compose
5. run wsl, go to selenoid and run docker-compose stack
   docker pull selenoid/video-recorder:latest-release
   docker pull selenoid/vnc:chrome_95.0
   docker-compose up -d
6. selenium server url now is localhost:4444
7. selenoid webui now is http://localhost:8080

