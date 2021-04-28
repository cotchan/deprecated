---
title: Flutter) MVVM에서 StateEnum을 사용하여 상태 관리하는 법 (doStatementChangedWork)
author: cotchan
date: 2021-04-23 23:44:00 +0800
categories: [Flutter, Flutter_main]
tags: [flutter2]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. INTRO

 

---

## 2. doStatementChangedWork

+ **viewModel에서 doStatementChangedWork를 정의하여 `none`, `doing`, `done`에 대한 상태를 분기합니다.**
  + **대표적인 사용처: `Progress bar`
    + `doing`일 때만 보여주고, none, done일 때는 보여주지 않기**

+ **중요한 점**
  + **`doStatementChangedWork를 적용하면` 화면 갱신을 얘가 전담
    + 그러므로 `statementChangedWork function 부분은 비즈니스 로직만` 담기게 됩니다.**

```dart
enum TaskState { none, doing, done }

abstract class ViewModel with ChangeNotifier {
  /// ViewModel에서 Navigator 사용을 위한 Context 정보
  BuildContext context;
  TaskState taskState;
  ViewModel(this.context, [this.taskState = TaskState.none]);

  Future<dynamic> doStatementChangedWork(Future<dynamic> Function() statementChangedWork) async {

    this.taskState = TaskState.doing;
    notifyListeners();

    final dynamic result = await statementChangedWork();

    this.taskState = TaskState.done;
    notifyListeners();

    return result;
  }
}
```

---

## 3. doStatementChangedWork 사용 예시

+ **`doProcess`**

```dart
//doProcess

  Future<void> doProcess(FileInfo fileInfo) async {

    doStatementChangedWork(() async {

      try
      {
        if (fileInfo.fileType == FileInfoType.dir)
        {
          _deleteCurrentPathFiles();
          _push(fileInfo.filePath);
          _files = await _getFileList(fileInfo.filePath);
        }
        else
        {
          _goDetails(fileInfo);
        }

      }
      on CommonException catch (e)
      {
        _isHaveException = true;
        _errorMessage = e.toString();
      }

    });

  }
```

---

+ **`init 함수`**

```dart
  Future<void> _init() async {

    doStatementChangedWork(() async {

      try
      {
        Directory appDir = await _storageFileRepository
            .getApplicationFilePath();
        _rootPath = appDir.path;

        print('rootPath = ${_rootPath}');
        _currentDirectoryName = '~/';

        _push(_rootPath);
        _files = await _getFileList(_rootPath);

      }
      on CommonException catch (e)
      {
        _isHaveException = true;
        _errorMessage = e.toString();
      }

    });
  }
```

---

## 4. progressBar 사용 예시


```dart
Widget _loadingIndicator() {
  if (viewModel.taskState == TaskState.doing) {
    return Center(
      child: SizedBox(
        child: CircularProgressIndicator(),
        height: 35,
        width: 35,
      ),
    );
  }
  else {
    return Container();
  }
}

return Scaffold(
  body: Stack(
    children: <Widget>[_draw(), _loadingIndicator()],
  ),
  backgroundColor: Colors.white,
);
```

---

+ 출처
  + [Provider State Management in Flutter](https://medium.com/codechai/provider-state-management-in-flutter-d453e73537c5)

