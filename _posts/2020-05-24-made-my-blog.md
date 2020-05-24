---
title:  "[Jekyll] 깃허브(Github) 블로그를 생성해 보자." 
excerpt: "Jekyll로 깃허브 블로그를 만들어 보았다."

categories:
  - Blog
tags:
  - [Blog, jekyll, Github, Git]
last_modified_at: 2019-05-23T08:06:00-05:00
---

🎀 [jekyll 한글 문서 페이지](https://jekyllrb-ko.github.io/) 🎀 를 참고하였다.

## 1. Github 에서 `블로그 용`으로 쓸 새로운 Repository 를 생성한다.

![image](https://user-images.githubusercontent.com/42318591/82748040-bd713b00-9dd9-11ea-8c65-4b54676abd1e.png)


레포지토리의 이름을 자신의 깃허브 계정 이름.github.io 로 하여 생성해준다.  
ex) `ansohxxn.github.io`


## 2. 생성한 Repository를 Local 환경으로 Clone 해 온다.

###### 1. 명령 프롬프트 cmd 를 실행하여 원하는 위치로 이동한다.
나의 경우 D드라이브에 설치하기 위해 cmd에 `D:` 를 입력 후 엔터 쳐 D드라이브로 이동하였다.
`cd` 명령어로 원하는 폴더 위치로 이동이 가능함 ! 원하는 폴더로 이동했다면 이제 이 로컬(내 컴퓨터) 폴더에 위에서 만든 레포지토리를 복사해 받아올 것이다.  


###### 2. git clone 명령어를 실행하여 레포지토리를 복사해온다.

    🔔 git이 미리 설치되어 있어야 한다. 
**`git clone` + 새 레포지토리 주소.git**
git clone 뒤에 위에서 만든 새 레포지토리의 주소, 그리고 `.git` 까지 붙여 명령어를 실행해준다.  
ex) `git clone https://github.com/ansohxxn/ansohxxn.github.io.git`  
이제 cmd상 현재 폴더 위치로 가보면 `깃허브아이디.github.io` 폴더가 생겨있을 것이다. 블로그로 쓸 레포지토리 복사 완료! 


## 3. Ruby 설치

    🔔 윈도우(Windows) 환경 기준

Jekyll은 Ruby라는 언어로 만들어졌기 때문에 jekyll을 설치하기 위해선 Ruby를 먼저 설치해야 한다고 한다.  루비 인스톨러 다운로드 페이지 <https://rubyinstaller.org/downloads/> 여기서 WITH DEVIKIT 중 가장 위에 있는 것을 다운받아 실행시킨다. 
  
✨ 인스톨러를 실행할때 여기 체크하면 직접 환경 변수 설정 해야 하는 수고로움을 생략할 수 있다.
- [ ] Add Ruby executables to your PATH  



## 4. Jekyll 과 Bundler 설치 

> <u>Bundler</u>는 루비 프로젝트에 필요한 gem들의 올바른 버전을 추적하고 설치해서 일관된 환경을 제공하는 도구이다.  

cmd에 다음과 같은 명령어를 수행한다. `gem install jekyll bundler`
cmd에 `jekyll -v` 명령어를 수행하여 jekyll이 잘 설치되었는지 확인해본다. 


## 5. jekyll 테마 

난 [minimal mistakes](https://github.com/mmistakes/minimal-mistakes) 테마를 선택했다. 이유는 많이 쓰이는 테마길래 정보가 많을 것 같아서..! 또한 기능도 많고 테마 색상도 여러가지길래 선택했다. 구글링 하면 jekyll 테마를 모아 둔 사이트가 여러개 나오는데 여러가지 구경해보다가 선택하게 되었다. 

선택한 jekyll 테마의 깃허브 레포지토리 주소를 복사하여 cmd에 **`git clone` + 선택한 jekyll 테마 레포지토리 주소.git** 명령어를 실행해 준다.
ex) git clone https://github.com/mmistakes/minimal-mistakes.git
