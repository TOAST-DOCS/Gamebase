# 문서 작성 가이드

본 문서는 가이드문서를 작성함에 있어서, 다음과 같은 것들을 설명합니다.

* 1. Markdown 문법 (Python-Markdown) : ReadTheDocs에 적용된 정적 페이지를 생성하는 빌더인 mkdocs에서 허용하는 Markdown 문법을 소개합니다.

* 2. Documentation 스타일 가이드 : 상기의 Markdown 문법을 이용하여, Documentation을 작성하면서, 어투나 문서를 작성하는 가이드라인을 소개합니다.




## 문서 구조
* root
	* Overview
	* Getting Started
	* Operator's Guide
	* Android Developer's Guide/Android Developer's Guide
	* iOS Developer's Guide/iOS Developer's Guide
	* Unity Developer's Guide/Unity Developer's Guide
	* unity-developer-guide
	* Server Developer's Guide
	* operator-guide


---



## 1. Markdown 문법

Mkdocs 에서는 Python-Markdown으로 작성된 문서를 빌드합니다.

Python-Markdown 링크 : [https://pythonhosted.org/Markdown](https://pythonhosted.org/Markdown)



---

## 2. Documentation 스타일 가이드



### 어조

- `경어`를 사용합니다. '-입니다.'와 같은 예사말은 금지합니다. '-입니다.'와 같이 작성합니다.

- 최대한 `간결`하고, 선언적으로 작성합니다. 간결함은 그 자체로 장점이 될 수 있습니다.

- `현재형`으로 작성합니다.

- 영문 문장이나 고유명사의 첫글자는 가능한한 `대문자`로 표기하도록 합니다.

- 문서를 업로드하기 전, `맞춤법 검사`를 수행하도록 합니다. (링크 : http://speller.cs.pusan.ac.kr/PnuSpellerISAPI_201602/)

- "HTML Markup Text"는 최대한 지양하되, `<dl><dt>..</dt><dd>..</dd></dl>`의 용어를 정의하는 것과 같은 Markdown에서 지원하지 않는 경우는 허용합니다.



### 이미지 등 미디어 삽입 경로

원칙적으로, 해당 도큐먼트가 위치한 폴더내의 `assets/` 이라는 폴더에 해당 이미지를 업로드하고, 상대경로를 이용하여 이미지 등의 미디어를 로드합니다. 추후 replace를 통하여 원격 FTP 경로의 이미지로 교체하여 토스트클라우드 문서에 업로드 합니다.

(다만, 공통적으로 사용될 수 있는 미디어의 경우는 루트경로에 있는 assets 폴더에 미디어를 업로드하고, 사용합니다)



1. 이미지 파일을 준비합니다.

2. 올릴 이미지를 해당 문서가 있는 폴더의 assets 폴더 내부로 이동/복사를 합니다.





3. 다음과 같은 마크다운을 이용하여, 이미지를 렌더링합니다.



`![image alt](assets/example_img3.png)`





### 문서 예제



```
## {Pitfall}

{다음의 내용은 사용자에게 조심해야할 사항을 보여줍니다.}

`주의` : `아래 그림의 빨간색 네모 부분을 꼭 채워서 입력하여주세요.`


## Table

| 1 | 2 | 3 | 4 |
| --- | --- | --- | --- |
| a |  |  |  |
|  | b |  |  |
|  |  | c |  |
|  |  |  | d |

```



----
