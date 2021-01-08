---
title: macOS) Launcher 사용방법
author: cotchan
date: 2021-01-08 10:19:21 +0800
categories: [Macos, Macos_ETC]
tags: [macos]
---

## 0. **목차**

1. **intro**
2. **LauncherControl**
3. **launchd Command**
    1. (로드 된) 사용 가능한 작업에 대한 정보 가져오기
    2. 주어진 작업에 대한 정보 얻기
    3. 작업 LOAD / UNLOAD 
    4. (로드 된) 작업 시작
    5. (로드 된) 작업 중지
    6. (로드 된) 작업 재시작
4. **A Sample Launchd Tutorial**
    1. (실행할) 프로세스 파일 생성
    2. launchd configuration plist file 생성
    3. launchctl 실행
    4. launchctl 중지

---

## 1. intro

- launchd는 macOS에서 데몬, 애플리케이션, 프로세스 및 스크립트를 시작, 중지 및 관리합니다.
- launchd는 데몬과 에이전트를 구분합니다.
    - 데몬은 항상 백그라운드에서 실행되는 시스템 전체 서비스를 의미합니다.
    - 에이전트는 사용자별 이벤트에서 실행되는 일반 서비스를 의미합니다.
- launchd.plist란
    - 시스템 전체 & 사용자별 데몬/에이전트 구성 파일

---

## 2. LauncherControl

- The launchd GUI
- You can enable or disable services with a single click.
- The same goes for loading, unloading and ad-hoc starting.

---

## 3. launchd Command

- 데몬으로 작업하는 경우 아래 명령과 함께 `sudo` 권한을 사용해야 합니다.

- (로드 된) 사용 가능한 작업에 대한 정보 가져오기

    ```bash
    $ launchctl 목록
    ```

- **주어진 작업에 대한 정보 얻기**

    ```bash
    $ launchctl 목록 | grep <LABEL>
    ```

- **작업 LOAD / UNLOAD (글로벌 데몬)**

    ```bash
    //load
    $ launchctl load /Library/LaunchDaemons/<LABEL>.plist

    //unload
    $ launchctl unload /Library/LaunchDaemons/<LABEL>.plist
    ```

- **(로드 된) 작업 시작**

    ```bash
    $ launchctl start <LABEL> 
    ```

- **(로드 된) 작업 중지**

    ```bash
    $ launchctl stop <LABEL>
    ```

- **(로드 된) 작업 다시 시작**

    ```bash
    $ launchctl restart <LABEL>
    ```

---

## 4. A Sample Launchd Tutorial

1. (실행할) 프로세스 **파일 생성**

    ```bash
    touch ~/demo/main.js

    # in main.js
    console.log("Hello", Date.now())
    ```

2. **launchd configuration plist file 생성**

    ```xml
    touch ~/Library/LaunchAgents/com.demo.daemon.plist

    # in com.demo.daemon.plist
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
      <dict>

        <key>Label</key>
        <string>com.demo.daemon.plist</string>

        <key>RunAtLoad</key>
        <true/>

        <key>StartInterval</key>
        <integer>20</integer>

        <key>StandardErrorPath</key>
        <string>/Users/{YOUR_NAME}/demo/stderr.log</string>

        <key>StandardOutPath</key>
        <string>/Users/{YOUR_NAME}/demo/stdout.log</string>

        <key>EnvironmentVariables</key>
        <dict>
          <key>PATH</key>
          <string><![CDATA[/usr/local/bin:/usr/local/sbin:/usr/bin:/bin:/usr/sbin:/sbin]]></string>
        </dict>

        <key>WorkingDirectory</key>
        <string>/Users/{YOUR_NAME}/demo</string>

        <key>ProgramArguments</key>
        <array>
          <string>/usr/local/bin/node</string>
          <string>main.js</string>
        </array>

      </dict>
    </plist>
    ```

    - 위 파일은 몇 가지 사항을 지정합니다.
        1. 사용자가 로그인 할 때마다 데몬이 시작됩니다. `RunAtLoad`
        2. 20초마다 실행됩니다. `StartInterval`
        3. 일부 로그 파일로 출력됩니다. `StandardOutPath`
        4. 환경 경로를 설정합니다. `EnvironmentVariables`
        5. 이 명령은 /User/{YOUR_NAME}/demo 디렉토리에서 실행됩니다. `WorkingDirectory`
        6. 그리고 명령은 /usr/local/bin/node main.js 입니다. `ProgramArguments`

    - **XML 각 key의 의미**
        1. `**Label**`
            - 이 키는 실행하고자하는 작업을 정의할 때 필요합니다. (모든 작업 정의에 필요)
            - 작업을 식별하는 용도로 사용합니다.

                ```xml
                <key>Label</key>
                <string>com.demo.daemon.plist</string>
                ```

        2. `**RunAtLoad**` **(boolean)**
            - 이 키는 작업이 로드되는 즉시 실행시키는 용도로 사용합니다.
                - 작업이 데몬이라면, 부팅 시 실행을 의미합니다.
                - 작업이 에이전트라면, 로그인한 시점을 의미합니다.

                ```xml
                <key>RunAtLoad</key>
                <true/>
                ```

        3. `**StartInterval**`
            - N초마다 실행하려는 작업이 있는 경우 이 키를 사용합니다.

                ```xml
                <!-- 한 시간마다 작업을 실행하려는 경우 -->

                <key>StartInterval</key>
                <integer>3600</integer>
                ```

        4. `**StandardErrorPath` & `StandardOutPath` & `StandardInPath`**
            - input & output을 내보내는 경로입니다. (Redirecting input and output)
            - 위 키들은 문자열을 인수로 사용합니다. 가능하면 절대 경로를 제공해야 합니다.
                - 상대 경로는 RootDirectory 또는 / 를 기준으로 해석됩니다.
        5. `**EnvironmentVariables**`
            - 이 키를 사용하여 프로그램이 실행되는 환경을 사용자 지정합니다.
                - 환경변수, 작업 디렉토리, 표준 입력 및 출력을 설정합니다.
                - 또한 코어 파일 생성을 활성화하거나 실행 파일이 가져오는 CPU 시간을 제한합니다.

                ```xml
                <key>EnvironmentVariables</key>
                <dict>
                	<key>PATH</key>
                	<string>/bin:/usr/bin:/usr/local/bin</string>
                </dict>
                ```

        6. `**WorkingDirectory**`
            - 이 키를 사용하여 프로그램 작업 디렉토리를 설정할 수 있습니다.
            - 실행 파일이 엑세스하는 모든 상대 경로는 이 디렉토리 기준입니다.

                ```xml
                <key>WorkingDirectory</key>
                <string>/tmp</string>
                ```

        7. `**ProgramArguments**`
            - command line Option이 필요하면 사용합니다.

                ```xml
                <key>ProgramArguments</key>
                <array>
                	<string>/usr/bin/rsync</string>
                	<string>--archive</string>
                	<string>--compress-level=9</string>
                	<string>/Volumes/Macintosh HD</string>
                	<string>/Volumes/Backup</string>
                </array>
                ```

            - 위 XML은 아래 shell command와 동일합니다.

                ```bash
                /usr/bin/rsync --archive --compress-level=9 "/Volumes/Macintosh HD" "/Volumes/Backup"
                ```

        8. `**KeepAlive**`
            - launchd는 특정 조건에 따라 작업이 실행되고 있는지 확인하는 데 사용할 수 있습니다.
            - 작업 정의가 로드되자마자 작업을 실행시킵니다. (RunAtLoad)
                - 무슨 작업이든간에, launchd가 작업을 로드한 후 즉시 실행합니다.
            - 작업이 중단되면 재시작시킵니다.
                - launchd는 재시작 사이에 ThrottleInterval 초를 기다립니다.

                ```xml
                <key>KeepAlive</key>
                <true/>
                ```

        - **더 많은 KEY 정보는 [https://www.launchd.info/](https://www.launchd.info/)에서 확인할 수 있습니다.**

3. **launchctl 실행**

    ```bash
    $ launchctl load ~/Library/LaunchAgents/com.demo.daemon.plist
    ```

    - `~/demo/stdout.log`

        ```bash
        demo % cat stdout.log 

        Hello 1610008745738
        Hello 1610008765791
        Hello 1610008785840
        Hello 1610008805890
        Hello 1610008825940
        Hello 1610008845989
        Hello 1610008866039
        Hello 1610008886088
        ```

4. **launchctl 중지**

    ```bash
    $ launchctl unload ~/Library/LaunchAgents/com.demo.daemon.plist
    ```

---

- 출처
    - [https://www.soma-zone.com/LaunchControl/](https://www.soma-zone.com/LaunchControl/)
    - [https://medium.com/swlh/how-to-use-launchd-to-run-services-in-macos-b972ed1e352](https://medium.com/swlh/how-to-use-launchd-to-run-services-in-macos-b972ed1e352)
    - [https://medium.com/@chetcorcos/a-simple-launchd-tutorial-9fecfcf2dbb3](https://medium.com/@chetcorcos/a-simple-launchd-tutorial-9fecfcf2dbb3)
    - [https://www.launchd.info/](https://www.launchd.info/)
