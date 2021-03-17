---
layout: post
title: Running PHPStorm & Docker in WSL 2
---

The goal of this article is mainly to share my excitement about WSL 2 and showcase how I use it as my local development environment.

## Introduction Note

The first thing to say is that WSL 2 is bringing kickass performance to the game. I’ve tried Docker for Windows without WSL, with WSL 1 and WSL 2. The latest is definitely the most performant and most usable, by far (while I did not do any benchmark of my own, don’t hesitate to search for benchmark by yourself).  
  
With WSL 2 you will have a linux environment where you can clone your repositories, enjoy the linux file permissions and still have the ability to access all the files from the windows explorer (using a network drive \\wsl$\Ubuntu-20.04).
  
To enjoy the best performance, you will want to use the “Linux File System”, meaning working with your files directly in WSL and not as a mounted folder from windows.
  
## The benefits
- Code hosted on linux
- PHPStorm running on linux
- Docker running on linux
- All that while working on Windows 😊

## The steps you will need to achieve are:
1. Install WSL 2
2. Install Docker Desktop (+ enable WSL 2 in the settings)
3. Install a X-Server on Windows
4. Open a terminal and start WSL
5. Install PHPStorm in linux
6. Create a shortcut on windows to launch PHPStorm in WSL 2  

#### 1. Install WSL 2  
Find all the information on [how to install WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

#### 2. Install Docker Desktop  
Download & Install [Docker Desktop](https://www.docker.com/products/docker-desktop)

#### 3. Install a X-Server on Windows  
I personally use [vcxsrv](https://sourceforge.net/projects/vcxsrv/) but you can use any X-Server you want.  

#### 4. Open a terminal and start WSL
This is an easy step, simply open any command line and execute `"wsl"`
![an image alt text]({{ site.baseurl }}/images/posts/wsl-command.png "an image title")

#### 5. Install PHPStorm in linux  
Good luck!  

#### 6. Create a shortcut on Windows to launch PHPStorm in WSL 2

Create the helper that will execute the command inside WSL.  
_C:\Users\%USERNAME%\programs\linux-apps\wsl-app-runner.bat_
```visualbasic
@echo off
for /f "tokens=3 delims=: " %%I in ('netsh interface IPv4 show addresses "vEthernet (WSL)" ^| findstr /C:"IP Address"') do set ip==%%I
set ipAddress=%ip:~1%
Powershell.exe wsl "DISPLAY='%ipAddress%':0" %1
```

Create the script to start the PHPStorm executable in WSL.  
_C:\Users\%USERNAME%\programs\linux-apps\phpstorm.vbs_  
```visualbasic
Set oShell = CreateObject ("Wscript.Shell")
Dim strArgs
strArgs = "cmd /c wsl-app-runner.bat /opt/PhpStorm-203.5981.175/bin/phpstorm.sh"
oShell.Run strArgs, 0, false
```

Create a Windows shortcut to start PHPStorm from the Windows menu.  
_C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\PHPStorm Ubuntu-20.04_
```

```
  

Make sure your x-server is started and properly configured and you should now be able to start PHPStorm in WSL from Windows.  

