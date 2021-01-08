---
title: macOS) Daemon & Agent
author: cotchan
date: 2021-01-08 13:55:21 +0800
categories: [Macos, Macos_ETC]
tags: [macos]
---


## 0. 목차

1. Intro
2. Daemon
3. Agent
4. 데몬(daemon)과 에이전트(agent)의 주요 차이
5. Job Definitions
6. Automatic Loading of Job Definitions

---

## 1. Intro

- Daemon? Agent?
    - Background program to perform
        1. To actions without user interaction
        2. To manage a shared state between multiple other programs

---

## 2. Daemon

- A program that runs in the background as part of the overall system
    - not tied to particular user
    - cannot display any GUI

- 현재 권장되는 Daemon 실행(시작) 방식은 `launchd`를 통한 실행방법 한 가지 뿐입니다.
- launchd daemon에 있는 property-list file(`launchd.plist`)을 사용하면 다양한 기준에 따라 데몬을 시작할 수 있습니다.

---

## 3. Agent

- `특정 사용자`를 대신해서 백그라운드에서 실행되는 프로세스
- 에이전트는 데몬이 할 수 없는 작업을 수행할 수 있습니다.
    - 사용자의 홈 디렉토리에 안정적으로 엑세스 하기
    - 윈도우 서버에 연결하기

---

## 4. 데몬(daemon)과 에이전트(agent)의 주요 차이

1. **GUI**
    - 데몬은 GUI 표시 `불가능`
    - 에이전트는 원하는 경우 GUI 표시 `가능`
2. **Window Server 연결** 
    - 데몬은 Window Server 연결 불가능
    - 에이전트는 Window Server 연결 가능
3. **작업 수준(Level)**
    - [https://www.techrepublic.com/article/macos-know-the-difference-between-launch-agents-and-daemons-and-use-them-to-automate-processes/](https://www.techrepublic.com/article/macos-know-the-difference-between-launch-agents-and-daemons-and-use-them-to-automate-processes/)
    - 데몬은 System level에서 작업을 실행합니다.
    - 에이전트는 사용자의 대화형 세션 컨텍스트 내에서 작업을 실행합니다.
        - the context of the user's interactive session
    - `시스템 수준의 엑세스`에 의존하는 작업이, `필요한 권한이 없는 사용자 공간`에서 실행되지 않도록 처리해야하는 게 중요한 이슈입니다.
4. **누구를 대신해서 실행?**
    - [https://www.launchd.info/](https://www.launchd.info/)
    - 데몬은 `루트 사용자` 또는 `UserName` key로 지정한 사용자를 대신해서 데몬이 실행됩니다.
    - 에이전트는 `로그인 한 사용자`를 대신하여 에이전트가 실행됩니다.
5. **호출시점**
    - 데몬은 시스템 시작 시에 호출
        - **LaunchDaemons**
            - 일반적으로 시스템이 부팅될 때 시작
            - 특정 사용자 세션 외부에서 실행
    - 에이전트는 사용자 로그인 시점에 호출
        - **LaunchAgents**
            - 사용자가 그래픽 세션에 로그인 할 때만 호출됩니다.

---

## 5. Job Definitions

- [https://www.launchd.info/](https://www.launchd.info/)
- 데몬 / 에이전트의 동작은 `property list`라는 특수 XML 파일에 저장됩니다.
- `저장 위치`에 따라 데몬 또는 에이전트로 처리됩니다.

    ![Desktop View](/assets/img/post/macos/2021-01-08-macos-daemonAndAgentType.png)

1. **Label**
    - Naming your job
    - 각 작업을 식별하는 정보입니다. (It identifies the job)
2. **Program / ProgramArgument**
    - 실행할 작업을 정의합니다. (This key defines what to start)
    - A valid job definition requires at least one these keys

        ```xml
        <key>Program</key>
        <string>/path/to/program</string>

        <key>ProgramArguments</key>
        <array>
        	<string>/usr/bin/rsync</string>
        	<string>--archive</string>
        	<string>--compress-level=9</string>
        	<string>/Volumes/Macintosh HD</string>
        	<string>/Volumes/Backup</string>
        </array>
        ```

3. **RunAtLoad** (Boolean)
    - 이 키는 작업이 로드되는 즉시 시작시키기 위해 사용합니다.
    - This key is used to start the job as soon as it has been loaded.

        ```xml
        <key>RunAtLoad</key>
        <true/>
        ```

    - **true인 경우**
        1. daemon은 `부팅 시점`을 의미
        2. agent의 경우 `USER 로그인 시점`을 의미

            ```xml
            <?xml version="1.0" encoding="UTF-8"?>
            <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
            <plist version="1.0">
            	<dict>
            		<key>Label</key>
            		<string>com.example.app</string>
            		<key>Program</key>
            		<string>/Users/Me/Scripts/cleanup.sh</string>
            		<key>RunAtLoad</key>
            		<true/>
            	</dict>
            </plist>
            ```

---

## 6. Automatic Loading of Job Definitions

- **Daemon Case**
    - 시스템 시작 시 `루트 실행 프로세스`는 `데몬 디렉토리`에서 작업 정의를 검색하고, `Disabled key`의 존재, 값 및 재정의 데이터베이스의 내용에 따라 로드합니다
        - /System/Library/LaunchDaemons
        - /Library/LaunchDaemons
- **Agent Case**
    - 사용자가 로그인을 하면 사용자에 대해 새로 시작된 프로세스가 시작됩니다.
    - 이 시작된 프로세스는 `에이전트 디렉토리`에서 작업 정의를 검색하고, `Disabled key`의 존재, 값 및 재정의 데이터베이스의 내용에 따라 로드합니다
        - /System/Library/LaunchAgents
        - /Library/LaunchAgents
        - ~/Library/LaunchAgents
- **Loading and Starting**
    1. 작업 정의를 로드한다고 해서 반드시 작업을 시작하는 건 아닙니다.
    2. `작업 시작 시기`는 `작업 정의`에 의해 결정됩니다.
    3. 실제로 `RunAtLoad` 또는 `KeepAlive`가 지정된 경우에만 launchd가 로드되었을 때 무조건 작업을 시작합니다.

---

- 출처
    - [https://www.launchd.info/](https://www.launchd.info/)
    - [https://apple.stackexchange.com/questions/290945/what-are-the-differences-between-launchagents-and-launchdaemons](https://apple.stackexchange.com/questions/290945/what-are-the-differences-between-launchagents-and-launchdaemons)
    - [https://www.techrepublic.com/article/macos-know-the-difference-between-launch-agents-and-daemons-and-use-them-to-automate-processes/](https://www.techrepublic.com/article/macos-know-the-difference-between-launch-agents-and-daemons-and-use-them-to-automate-processes/)
