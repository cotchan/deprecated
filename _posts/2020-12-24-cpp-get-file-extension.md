---
title: C++) 파일 확장자명 구하기(Feat. std::wstring)
author: cotchan # AUTHOR
date: 2020-12-24 09:33:21 +0800
categories: [C++, C++_Tip]
tags: [c++]
---

## 확장자명 구하는 방법

```c++
std::wstring getFileExtension(const wchar_t* filePath)
{
    if (nullptr == filePath)
    {
        return;
    }
    
    std::wstring strFilePath = filePath;
    std::wstring::size_type len = strFilePath.length();
    std::wstring::size_type dot_pos = strFilePath.rfind(L'.');
	
    //example) _ext == L".txt", L".pdf"
    std::wstring _ext = strFilePath.substr(dot_pos, len - dot_pos);

    return _ext;	
}


//function call
int main(void)
{
    std::wstring param = L"test.docx";

    //ext == L".docx" 
    std::wstring ext = getFileExtension(param.c_str());

    return 0;
}
```
---

