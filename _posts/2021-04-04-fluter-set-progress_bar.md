---
title: Flutter) [Module] Progress bar 사용방법
author: cotchan
date: 2021-04-04 21:44:00 +0800
categories: [Flutter, Flutter_module]
tags: [flutter2]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Progress Bar Logic 요약

```dart
class _MainPageState extends State<MainPage> {

  TextEditingController _stationController = TextEditingController(text: api.defaultStation);
  List<SubwayArrival> _data = [];
  bool _isLoading = false;   //55555

  @override
  void initState() {
    super.initState();
    _getInfo();
  }

  _onClick() {
    _getInfo();
  }

  _getInfo() async {

    //초기 셋팅
    setState( () => _isLoading = true);   //12 12 12 12 12

    //초기화

    //예외 상황
    if (errorMessage['status'] != api.STATUS_OK)
    {
      setState(() {
        final String errMessage = errorMessage['message'];
        print('error >> $errMessage');
        _data = const [];
        _isLoading = false;
      });
      return;
    }


    //정상 로직일 때

    //정상 로직 완료 후
    setState(() {   //14 14 14 14 14
      _data = list;
      _isLoading = false;
    });
  }
}
```

---

- **`55555`**
    - **`_isLoading` 변수는 공공 API를 조회할 때 프로그레스 바를 띄우기 위해 사용합니다.**
    - **`_isLoading`이 true면 원형 프로그레스 바를 띄우고, 결과 조회가 완료되면 _isLoading 변수를 false로 바꾸고 결과를 표시합니다.**

- **`12 12 12 12 12`**
    - `_getInfo()` 메서드의 내부입니다.
    - 데이터를 조회 중에는 `_isLoading` 변수를 true로 만듭니다.
    - 그러면 원형 프로그레스 바가 표시됩니다.
    - 프로그레스 바 표시에 대해서는 `build()` 메서드에서 설명합니다.

- **`14 14 14 14 14`**
    - 만약 정상적인 데이터가 나오면
    - `list` 변수에 데이터를 담고
    - `_data`에 데이터를 넣고
    - `_isLoading` 변수를 false로 한 후에
    - `setState()` 메서드를 호출합니다.
---

## 2. Progress Bar 사용 예시 코드

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';
import 'model/subway_arrival.dart';   
import 'api/subway_api.dart' as api;  

void main() => runApp(SubwayDemo());

class SubwayDemo extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '지하철 실시간 정보',
      debugShowCheckedModeBanner: false,
      home: MainPage(),
    );
  }
}


class MainPage extends StatefulWidget {
  @override
  _MainPageState createState() => _MainPageState();
}

class _MainPageState extends State<MainPage> {

  /**
   * part1. 클래스의 멤버 변수와 _buildCards() 메서드
   */
  TextEditingController _stationController = TextEditingController(text: api.defaultStation);
  List<SubwayArrival> _data = [];   
  bool _isLoading = false;    //55555

  List<Card> _buildCards() {
    print('>>> _data.length? ${_data.length}');

    if (_data.length == 0) {    
      return <Card>[];
    }

    List<Card> res = [];
    for (SubwayArrival info in _data)   
    {
      Card card = Card(
        child: Column(
          children: <Widget>[
            AspectRatio(
              aspectRatio: 18 / 11,
              child: Image.asset(
                'assets/icon/subway.png',
                fit: BoxFit.fitHeight,
              ),
            ),
            Expanded(   
                child: Padding(
                  padding: EdgeInsets.fromLTRB(16.0, 12.0, 16.0, 8.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.center,
                    children: <Widget>[
                      Text(
                        info.trainLineNm,
                        maxLines: 1,
                        overflow: TextOverflow.ellipsis,
                      ),
                      SizedBox(height: 4.0),
                      Text(
                        info.arvlMsg2,
                        maxLines: 1,
                        overflow: TextOverflow.ellipsis,
                      ),
                    ],
                  ),
                )
              )
          ],
        ),
      );
      res.add(card);    
    }

    return res;
  }

  /**
   * part2. initState(), _onClick(), _getInfo() 메서드
   */
  @override
  void initState() {    
    super.initState();
    _getInfo();
  }

  _onClick() {   
    _getInfo();
  }

  _getInfo() async {
    setState( () => _isLoading = true);   //12 12 12 12 12

    String station = _stationController.text;
    var response = await http.get(api.buildUrl(station));
    String responseBody = response.body;
    print('res >>> $responseBody');

    var json = jsonDecode(responseBody);
    Map<String, dynamic> errorMessage = json['errorMessage'];

    if (errorMessage['status'] != api.STATUS_OK)    
    {
      setState(() {
        final String errMessage = errorMessage['message'];
        print('error >> $errMessage');
        _data = const [];
        _isLoading = false;
      });
      return;
    }

    List<dynamic> realtimeArrivalList = json['realtimeArrivalList'];
    final int cnt = realtimeArrivalList.length;

    List<SubwayArrival> list = List.generate(cnt, (int i ) {
      Map<String, dynamic> item = realtimeArrivalList[i];
      return SubwayArrival(
          item['rowNum'],
          item['subwayId'],
          item['trainLineNm'],
          item['subwayHeading'],
          item['arvlMsg2']
      );
    });

    setState(() {   //14 14 14 14 14
      _data = list;
      _isLoading = false;
    });
  }

  /**
   * part3. 데이터 표시 부분
   * 앞에서 만든 내용을 바탕으로 카드를 표시합니다.
   */
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      resizeToAvoidBottomPadding: false,
      appBar: AppBar(
        title: Text('지하철 실시간 정보'),
      ),
      body: _isLoading    //15 15 15 15 15
          ? Center(child: CircularProgressIndicator())
          : Column(
              mainAxisAlignment: MainAxisAlignment.start,
              crossAxisAlignment: CrossAxisAlignment.start,
              children: <Widget>[
                Container(
                  padding: EdgeInsets.symmetric(horizontal: 20),
                  height: 50,
                  child: Row(
                    children: <Widget>[
                      Text('역 이름'),
                      SizedBox(
                        width: 10,
                      ),
                      Container(
                        width: 150,
                        child: TextField(
                          controller: _stationController,
                        ),
                      ),
                      Expanded(
                          child: SizedBox.shrink()
                      ),
                      RaisedButton(
                          child: Text('조회'),
                          onPressed: _onClick,
                      ),
                    ],
                  ),
                ),
                SizedBox(
                  height: 10,
                ),
                Padding(
                  padding: EdgeInsets.fromLTRB(20, 0, 0, 0),
                  child: Text('도착 정보'),
                ),
                SizedBox(
                  height: 10,
                ),
                Flexible(   
                    child: GridView.count(
                      crossAxisCount: 2,
                      children: _buildCards(),
                    ),
                ),
              ],
            ),
    );
  }
}
```

---

## 3. 사용 예시 코드 해석

- **`55555`**
    - **`_isLoading` 변수는 공공 API를 조회할 때 프로그레스 바를 띄우기 위해 사용합니다.**
    - **`_isLoading`이 true면 원형 프로그레스 바를 띄우고, 결과 조회가 완료되면 _isLoading 변수를 false로 바꾸고 결과를 표시합니다.**

- **`12 12 12 12 12`**
    - `_getInfo()` 메서드의 내부입니다.
    - 데이터를 조회 중에는 `_isLoading` 변수를 true로 만듭니다.
    - 그러면 원형 프로그레스 바가 표시됩니다.
    - 프로그레스 바 표시에 대해서는 `build()` 메서드에서 설명합니다.

- **`14 14 14 14 14`**
    - 만약 정상적인 데이터가 나오면
    - `list` 변수에 데이터를 담고
    - `_data`에 데이터를 넣고
    - `_isLoading` 변수를 false로 한 후에
    - `setState()` 메서드를 호출합니다.

- **`15 15 15 15 15`**
    - `_isLoading` 변수가 true면 원형 프로그레스 바(`CircularProgressIndicator()`)를 표시하고
    - 로딩이 완료되어 `_isLoading` 변수가 false면 `Column` 위젯으로 내용을 표시합니다.

---

## 4. 사용 예시 코드 Github 주소

+ **[http_json_sample_project/3-3.usingPublicDataApi_v3](https://github.com/cotchan/http_json_sample_project/tree/feature/3-3.usingPublicDataApi_v3)**

---

+ 출처
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 

