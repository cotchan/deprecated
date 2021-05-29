---
title: Flutter) onChanged ObscureText
author: cotchan
date: 2021-05-26 21:43:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Intro

+ **비밀번호 Field가 버튼 클릭에 따라 `obscureText 처리 되었다가 안 되었다가를 반복`하는 코드 완성** 

---

## 2. 시나리오 정리

1. **버튼을 눌렀을 때 `onPressed`가 실행되면서 widget.obscureText 값이 바뀜**
2. **바뀐 값은 onChangedObscureText: (`value`)에 들어오면서 이 `value`값이 viewModel.updateObscureState 메서드의 `파라미터로 들어감`**
3. **파라미터로 들어간 value는 updateObscureState 메서드 내부에서 `_isObscureText 값으로 셋팅되고` notifyListeners 를 통해 화면의 obscureText 플래그 값이 갱신됨**
4. **3번에 따라서 PasswordTextFormField 필드의 obscureText 플래그가 바뀌면서, 화면에 * 처리가 되었다가, 다시 보이는 형태로 바뀜**

---

## 3. view.dart

+ **`ValueChanged<bool>` 선언을 통해 bool 값을 가지고 갈 수 있도록 선언** 

```dart
//view.dart
ValueChanged<bool>? onChangedObscureText;


//Usage
Widget _drawPasswordForm() {
  return PasswordTextFormField(
    //...
    obscureText: viewModel.isObscureText,
    onChangedObscureText: (value) => viewModel.updateObscureState(value),
  );
}
```

---

## 4. viewModel.dart

```dart
//viewModel.dart
bool _isObscureText = true;
bool get isObscureText => _isObscureText;

void updateObscureState(bool value) {
  _isObscureText = value;
  notifyListeners();
}
```

---

## 5. form.dart

```dart
//form.dart
class _PasswordTextFormFieldState extends State<PasswordTextFormField> {
  //...
  onPressed: () => {
    setState(() => {
      widget.onChangedObscureText!(!widget.obscureText),
    }),
  },
  //...
}
```

---

+ 출처

