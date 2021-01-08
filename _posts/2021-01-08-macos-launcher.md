---
title: macOS) Launcher 사용방법
author: cotchan
date: 2021-01-08 10:19:21 +0800
categories: [Macos, Macos_ETC]
tags: [macos]
---

**<목차>**

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

- `launchd`는 macOS에서 데몬, 애플리케이션, 프로세스 및 스크립트를 시작, 중지 및 관리합니다.
- launchd는 데몬과 에이전트를 구분합니다.
    - 데몬은 항상 백그라운드에서 실행되는 시스템 전체 서비스를 의미합니다.
    - 에이전트는 사용자별 이벤트에서 실행되는 일반 서비스를 의미합니다.
- `launchd.plist`란
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

    ```cpp
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

1. (실행할) **`프로세스 파일` 생성**

    ```bash
    touch ~/demo/main.js

    # in main.js
    console.log("Hello", Date.now())
    ```

2. **launchd configuration `plist file` 생성**
    ```bash
    touch ~/Library/LaunchAgents/com.demo.daemon.plist
    ```

    ```bash
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
        <string>/Users/chajunho/demo/stderr.log</string>

        <key>StandardOutPath</key>
        <string>/Users/chajunho/demo/stdout.log</string>

        <key>EnvironmentVariables</key>
        <dict>
          <key>PATH</key>
          <string><![CDATA[/usr/local/bin:/usr/local/sbin:/usr/bin:/bin:/usr/sbin:/sbin]]></string>
        </dict>

        <key>WorkingDirectory</key>
        <string>/Users/chajunho/demo</string>

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
        5. 이 명령은 /User/{USER_NAME}/demo 디렉토리에서 실행됩니다. `WorkingDirectory`
        6. 그리고 명령은 /usr/local/bin/node main.js 입니다. `ProgramArguments`

3. **launchctl 실행**

    ```bash
    $ launchctl load ${.plist}
    ```

    ```bash
    $ launchctl load ~/Library/LaunchAgents/com.demo.daemon.plist
    ```

    - `~/demo/stdout.log`

        ```bash
        $ cat stdout.log 

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
    $ launchctl unload ${.plist}
    ```

    ```bash
    $ launchctl unload ~/Library/LaunchAgents/com.demo.daemon.plist
    ```

---

- 출처
    - [https://www.soma-zone.com/LaunchControl/](https://www.soma-zone.com/LaunchControl/)
    - [https://medium.com/swlh/how-to-use-launchd-to-run-services-in-macos-b972ed1e352](https://medium.com/swlh/how-to-use-launchd-to-run-services-in-macos-b972ed1e352)
    - [https://medium.com/@chetcorcos/a-simple-launchd-tutorial-9fecfcf2dbb3](https://medium.com/@chetcorcos/a-simple-launchd-tutorial-9fecfcf2dbb3)
