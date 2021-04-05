FROM mcr.microsoft.com/windows/servercore:ltsc2019

# install needed fonts for googlechrome to run properly
ADD files/fonts.tar /Fonts/
WORKDIR /Fonts/
RUN @powershell -NoProfile -ExecutionPolicy Bypass -Command ".\Add-Font.ps1 Fonts"

WORKDIR "C:/ProgramData"

# enable Web-WebSockets
RUN @powershell -NoProfile -ExecutionPolicy Bypass -Command "Add-WindowsFeature Web-WebSockets"
# install chocolately
RUN @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin" -Y

# install openjdk8
RUN choco install openjdk8 -Y
# update environment variables
RUN refreshenv

# install googlechrome
RUN choco install googlechrome -Y

# copy your application to the WORKDIR
ADD files/myapplication.war "C:/ProgramData"
# copy chromedriver.exe to the same level of your application
# assumming your application 
ADD files/drivers/chromedriver.exe "C:/ProgramData/drivers"

EXPOSE 8080

# run your application
CMD ["java", "-jar", "C:/ProgramData/myapplication.war"]