---
title: "비개발자가 Jekyll, Github pages로 개인 웹사이트(블로그) 만든 과정"
categories:
  - Review
  - Knowledge
tags:
  - life
  - blog
  - website
  - guide
  - 일상
  - 블로그
  - 웹사이트
  - 가이드
  - jekyll
  - SSG
  - Github Pages
  - Github
excerpt:  "어느 날 삘 받은 비개발자가 Github pages와 Jekyll을 이용해서 개인 블로그와 웹사이트를 만들게 되기까지의 과정이다. This post shows a method to build a personal website or blog using Jekyll hosted by Github pages"
toc: true
toc_lable: "개인 블로그 만들기"
published: true
---

## Jekyll + Github pages로 블로그를 만들게 된 계기

### 어느날 떠오른 생각
{: .no_toc}

보통 세상의 많은 재미있는 일들은 준비과정이 지루하고 본 과정은 순식간에 끝나는 경향이 있는 반면, 어떤 일들은 오히려 준비과정이 훨씬 재미있는 경우가 있다. 학창시절 교과서 내용을 정리하고 필기하는 것보다 필기구를 종류별로 구비해놓는 것이 더 재미있다던지, 쓰지도 않을 요리재료를 이것저것 카트에 담으며 장을 본다던지 하는 등. 어쩌면 이 블로그도 그런 맥락에서 시작되었다.

가장 처음 들었던 생각은 이런거였다. 첫째는 그냥 *내가 가진 생각을 ~~개똥철학이지만~~ 뭔가 전시해두고 싶다* 라는 욕구에서 출발했고, 둘째는 *AI 시대에 데이터가 그렇게 부족해질텐데 뭔가 내가 기여할 수 있는 바가 있지 않을까 (특히 한국어 화자 입장에서)* 하는 생각이 들었고, 셋째는 *gene은 사라져도 meme은 남기고픈* 그런 욕망이 생겼던 것 같다.

그래서 그냥 막연하게 블로그나 한번 해볼까 하는 생각이 들었는데, 특유의 반골 기질이랄까 홍대병이라고 해야 할까. 그냥 평범한 네이버 블로그나 티스토리 등은 뭔가 마음에 들지 않았다. 물론 훌륭한 블로그 플랫폼이긴 하나... 개인적으로 가지고 있던 이미지 자체가 별로 좋지 않았고 (수많은 상업용 블로그, 애드센스 떡칠해놓은 영양가 없는 글들) 나는 그냥 대충 글만 싸지르고 싶은데 그렇게 거창한게 필요할까 싶은 생각이었다.

그럼 그냥 SNS에다가 가볍게 글을 올리거나, 다른 글쓰기 커뮤니티에 글을 쓰는 것은 어떨지 고민해봤다. (뭐, 씀 이라던지. 지금도 살아있는지는 모르겠다) 근데 아무래도 SNS는 거의 짧은 글에 최적화 되어 있기도 하고, 갑자기 수치심이 들었을 때 데이터 지우기도 불편한 데다가 이미 내 마음이 블로그 쪽으로 많이 기울어서 다른 블로그 플랫폼을 찾아봤다.

그렇게 알게 된게 Velog 인데... 심플하고 상업적 용도 사용 안 되는것도 마음에 드는데 이거 너무 개발자 특화 플랫폼이다...라는 생각이 들었다. 사실 나는 그냥 아무 글이나 (영양가나 컨텐츠 깊이와 관계없이) 자기만족 비슷하게 그냥 *써 제끼고* 싶은데 여기는 커뮤니티 특성도 좀 있는 건지 메인화면에 노출된 포스트들을 보니 개발자들이 유용하게 읽을 만한 글들이 잔뜩 있어서 그걸 보고 기가 죽었다. 그래서 바로 런.

좀 더 검색을 해 본 결과가 결국 Github pages 였다. 이 방식은 다음과 같은 장점이 있다:

1. 데이터베이스를 내가 관리함.
2. 로컬에서 작성해 뒀다가 서버에는 천천히 올릴 수 있음. (Vscode 띄워두고 코딩하는 척하면서 글쓰기 가능 ㅇ)
3. CSS 자유도 (하지만 나는 건드리지 않을 것)
4. 뻔하지 않은 새로운 방식이라 재밌음

그때는 몰랐다. 이게 이렇게 까지 할 게 많을 줄은...

## 시작하기 전에

아래부터는 내가 겪어왔던 시행착오를 그대로 정리하기 보다는, 결과적으로 지금 시점에 내가 이해하고 있는 사항을 한번 정리해 보려 한다.  

### 웹사이트/블로그 host/publish(deploy)에 대한 기본적인 개요

먼저 동적 웹사이트와 정적 웹사이트를 구분해야 한다. 자세한건 난 비개발자라 모르겠고, **서버단의 데이터베이스를 직접 활발하게 상호작용 할 일이 있으면 (로그인, 댓글 작성, 전자상거래 기능 등) 동적웹사이트, 그냥 Read-only 모드로 html/css 파일만 받아서 브라우저에 띄워주는 방식이면 정적 웹사이트** 라고 나는 이해했다.

> 동적 웹사이트는 아무튼 복잡한 기능. 
> 정적 웹사이트는 아무튼 데이터 읽기만 하면 되는 심플한 웹사이트.
{:  .notice--info}

나는 개인 블로그 게시 목적이니 정적 웹사이트를 배포하고자 한다고 이해하면 되겠다.

그럼 정적 웹사이트는 어떻게 만들어지고 게시되는가?  
단순하게 이야기하면 그냥 1) `.html`이랑 `.css`파일을 잘 작성해서 2) 호스팅 하면 된다. 그 뒤에 뭐 SEO(검색 엔진 등록 및 최적화)니 트래픽 분석이니 그건 상업용 블로그 하시는 분들한텐 중요할 터다.  
이제 1번 과정을 **제작** 과정, 2번 과정을 **배포** 과정이라고 하겠다.

### Jekyll이란?

#### 정적 웹사이트 생성기의 필요성 

제작 과정이란 그냥 니즈에 맞게 `.html` 파일과 `.css`파일을 작성하는 과정이다. 근데 파이썬 깔짝해본 비개발자 입장에서 이 문법을 죄다 알고 직접 이 파일을 작성하는건 불가능하지는 않겠지만 너무... 너무 귀찮은 일이다. 하물며 나는 Javascript도 모른다. 그래서 필요한 것이 정적 사이트 생성기 (SSG) 이다.  
그렇다. **Jekyll은 정적 웹사이트 생성기이다.** 정적 사이트 생성기 중 하나인 Jekyll의 경우, `.md` 파일을 마크다운 문법 (+ Liquid)에 맞게 작성하면 Jekyll이라는 이름의 일종의 프로그램이 출력물로 `html`파일과 `css`파일을 적절한 디렉토리 구조와 함께 뱉는다. 당연히 설치하고 설정을 해주는 과정이 좀 필요하긴 하나... ~~사실 이해하는데 더럽게 오래걸렸다~~ 어쨌든 편함. 암튼 편함.

## 웹사이트 제작과정

### Jekyll 설치하기

Jekyll 설치하기 전에 알아야 할 것은, 호스팅을 Github pages로 하기로 했으면 사실 로컬에 직접 Jekyll을 설치할 필요는 없다는 것이다. 왜냐면 그냥 소스파일만 허브에 올려도 Github 서버가 알아서 빌드하고 배포해주기 때문. 따라서 후술할 내용은 '굳이 로컬에서 먼저 내 블로그가 어떻게 변했는지 봐야겠다' 할 경우에만 해주면 된다. _의외로 그런게 필요할지도...?_  

> '로컬(인터넷 연결 없이)에서 배포될 웹사이트 결과물을 봐야겠다' 할때만 설치하기
{:  .notice--info}

1. Jekyll 설치 전, Ruby 설치
    Jekyll은 Ruby라는 언어로 구동된다. 따라서 Ruby를 설치해야 함. 참고로 생전 처음보는 언어였다. Rust면 모를까. 2025년 6월 기준으로 3.4.4 버전이 최신 안정버전이다. [Ruby 공홈 다운로드](https://www.ruby-lang.org/ko/downloads/)

    - 윈도우는 그냥 installer 실행하면 되서 쉽다. PATH 추가 잊지 않을 것. (installer가 알아서 해주니까 체크박스만 잘 읽고 체크하면 된다)
    - Mac은 homebrew가 있다는 전제하에 [rbenv](https://github.com/rbenv/rbenv?tab=readme-ov-file#installation) 깔아서 하면 된다. Homebrew가 안 깔려있을 수 있는데, 맥은 패키지 뭐 이용한다치면 homebrew 거의 필수니까 그냥 [바로 설치하자 (homebrew)](https://brew.sh/)  
    CLI나 터미널 사용이 익숙하지 않다? 맥 중고로 팔거 아니면 악으로 깡으로 익숙해져야함.  

    ``` bash
    % homebrew install
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

    ```

    > 잘 모르지만 rbenv가 ruby 가상환경을 관리해주는 툴인듯 싶다. 특히 맥은 권한 문제 때문에 그냥 글로벌에 바로 ruby를 설치해서 하려니까 잘 안되더라. `rbenv install (version)` 을 사용하자.
    {:  .notice--info}

2. Jekyll 설치
    최신 Ruby 기준으로 이미 `bundler`가 설치되어 있다. 그러므로 [Jekyll 공식 홈페이지](https://jekyllrb-ko.github.io/docs/)에서는 bundler를 설치하라고 되어 있지만 해도 안 해도 상관은 없다.

    이제 그냥 바로 터미널에다가 아래를 입력하자.

    ```  bash
    gem install jekyll
    ```

    그럼 끝!  
    원한다면 이제 프로젝트 의존요소 (dependencies) 관리용 gemfile을 생성하고 다음 과정을 진행하면 되지만... html 문법도 Ruby 문법도 모르는데 그걸 어케함?  
    다행히 세상의 능력자들이 이미 템플릿으로써 테마(Theme)를 고맙게도 무료로 배포하고 있다. 이걸 사용하자.

### Jekyll 테마 가져오기

그냥 검색창에 'jekyll Theme' 으로 검색하면 자료가 많이 나온다. 여기서는 내가 했던 방식을 거의 그대로 정리해두겠다. 참고로 나는 초보자 입장에서, 테마 이쁜것도 중요하지만 **문서 관리** 가 잘 되어 있는 테마를 우선적으로 골랐다. 사용자가 많고, 업데이트가 최근까지 이뤄졌으면서 디자인도 어느정도 마음에 드는 테마를 골라보니 chirpy랑 minimal-mistakes 두 개 정도가 눈에 띄었다. 일단은 [minimal-mistakes](https://mmistakes.github.io/minimal-mistakes/)로 결정.

#### 1. Minimal Mistakes 테마 가져오기

크게 세 가지 정도의 방법이 있다. Gem based method, remote theme method, 그리고 그냥 전체 파일 다운로드 방법.  
호스팅 서비스를 Github pages가 아닌 다른 호스팅을 이용할 경우 Gem-based method가 적절해보인다. 근데 이 방식은 초보자에게 다소 무리가 있는데, 왜냐면 `Gemfile` 과 `_config.yml` 파일의 수정을 요하기 때문이다. 참고로 `_config.yml` 파일은 jekyll로 웹사이트를 빌드할 때 필요한 설정들이 작성되어 있는 파일이다.  
그래서 나는 **Remote theme method**를 선택했다. 너무나 고맙게도 테마 제작자가 스타터 템플릿을 제공한다. 단, Github 저장소를 새로 만들어야 하니 Github 계정을 만들어야 한다. (무료이기 때문에 별 상관 없다.)

> [minimal-mistakes 테마 제작자가 제공하는 스타터 repo(?) 만들기](https://github.com/mmistakes/mm-github-pages-starter/generate)
{: .notice--primary}

> 이게 테마 제작자가 윈도우 사용자가 아닌건지 스타터 repo가 약간 outdated 되어 있다.
> `gem "wdm", "~> 0.2.0" if Gem.win_platform?` 
> 이부분의 wdm 버전을 0.2.0으로 수정해주자. (원래 뭐였는지 기억 안남)
{: .notice--warning}

만약 **통째로 다운로드**를 원한다면, 공식 문서에서 제공하는 zip파일을 제공받거나 아니면 [공식 repo](https://github.com/mmistakes/minimal-mistakes/tree/master)에서 fork 딸깍 눌러주면 된다. 이후에는 공식문서 지침을 따라 필요없는 파일을 지워주면 된다.  이 방식은 다 좋은데, 이후 테마 제작자가 업데이트를 했을때 좀 난감한 상황이 발생할 수 있다. 따라서 테마 제작자의 업데이트를 그대로 따라가면서 편하게 Github pages에 호스팅을 할 수 있는 방법은 역시 **Remote theme method**이다. 단, 이것은 minimal-mistakes 테마 제작자의 은혜이므로 다른 테마의 경우 다른 방식을 사용해야 할 수 있다. 그냥 fork 딸각 방식은 어디든 통하겠지만.

#### 2. 실험용 빌드 해보기

이제부터는 fork든 starter repo를 만들든 해서 테마 템플릿과 예시용 포스팅 소스 파일이 내 github 저장소에 저장되어 있다고 가정하고 진행해보자. 

1. Git repository clone하기
    `Github Desktop`을 쓰든, `Git bash`깔아서 `git clone`을 하든, 다른 서드파티 앱을 쓰든 어쨌든 저장소 파일을 내 로컬 디렉토리에 다운로드 받자.
2. 터미널을 열고 다운받은 디렉토리에 진입한 뒤 `bundle install` 을 통해 Gemfile내에 정의된 dependencies를 설치해주자. 이후 `bundle exec jekyll serve`를 터미널에 입력 후 실행. 그럼 로컬 환경에서 사이트가 빌드된다.
3. 서버 주소 `http:// 이후 숫자` 를 브라우저 검색창에 입력하거나 해서 주소에 진입하면 빌드된 사이트가 뜬다. 그럼 끝.

#### 3. 커스터마이징 하기

이제 대충 어떻게 돌아가는지는 파악이 되었을 것이다. `_posts` 디렉토리 하에 정해진 포맷 `YYYY-MM-DD-name-of-file.md` 이런 식으로 마크다운 파일을 만들어서 글을 작성하면 된다. 물론 여기에는 *YAML front matter* 라고 해서 일종의 파일 메타데이터를 입력해줘야 한다. (yaml 잘 모르지만 json 대신에 쓰는 느낌?? 중괄호 없는 key-value 매칭? 쨌든 보기 좋음) starter repo나 실제 테마 제작자의 포스터 파일을 참고하면 감을 잡을 수 있을 것이다. 가장 기본적인 형태는 다음과 같다.

``` markdown
---
title: "포스팅 제목"
categories:
  - 카테고리 1
  - 카테고리 2
tags:
  - 태그 1
  - 태그 2
excerpt:  "셀프 포스팅 내용 요약"
---

# 제목 1
내용 1

## 제목 2
내용 2
```

이정도만 알면 이제 기본적인 블로그 제작은 할 수 있다.  
그 외 자잘한 설정은 [테마 공식 문서](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)를 참고해서 수정해보자.

## 웹사이트 배포 과정

나는 `Github Pages`를 선택했지만, `Netlify`나 `Vercel`등도 인기가 있는 듯 하다. 특히 `Vercel`은 최신 프레임워크? `Next.js`? 비개발자라 잘 모르겠지만 암튼 까리하고 좋은 걸 적극적으로 사용한다는 듯하다. 나중에 시간나면 알아보자. 

### Github Pages로 배포하기

공식? 튜토리얼 같은게 있다. [튜토리얼](https://github.com/skills/github-pages)  
해보진 않았지만 도움이 될지도

#### 1. 가장 쉬운 방법 (Deploy from ...)

제일 쉽고 단순한 방식은 아까 fork하거나 만들었거나 했던 repo의 이름을 `<username>.github.io`로 변경하고, repo의 `설정(Settings)` 에 들어가서 `Pages` 항목을 찾은 뒤에 `Deploy from branch`로 마스터 브랜치를 배포하는 것이다. 참고로 무료 사용자는 repo가 public 상태여야 한다. 이후 아까 로컬에서 빌드했던 그 내용 그대로 commit 후 (로컬에 변경점 생성) push (내 로컬->Github 서버상의 내 repo로 데이터 덮어쓰기) 해주면 된다. 그리고 잠시 기다리면 알아서 빌드하고 배포해준다. 사이트 주소는 `https://<username>.github.io/` 이다.

#### 2. Custom Github Action workflow 이용하는 방법

처음에 repo의 이름을 `<username>.github.io`로 변경하고, repo의 `설정(Settings)` 에 들어가서 `Pages` 항목을 찾는 건 1번 방법과 동일하다. 이후 Deploy from branch 대신 `Github Action`을 선택하고, 템플릿 검색창에 `Deploy Jekyll site to Pages` 를 검색해서 이 템플릿을 그대로 사용해서 새로운 워크플로우를 추가하면 된다. 대신 이 방식은 `gh-pages`라는 branch를 따로 만들어줘야 한다. (`master` branch에 소스 파일을 push하면, 자동으로 Github 서버 측에서 빌드를 수행, 결과물을 `gh-pages` 브랜치에 push해주는 방식인 듯.) 이딴 짓 왜 하나 싶지만 몇 가지 장점이 있다고 한다. 디버깅이 쉽다던가, 커스텀 도메인을 사용할때 CNAME 파일을 만들 필요가 없다던가. 근데 아직 크게 체감은 안된다.

#### Github Pages로 호스팅할때 알아야 할 제한사항

몇 가지 꼭 알아야할 사항이 있다.  

1. **광고 제한** : 법 지켜야 되고 성적으로 외설적인 내용이나 폭력적인 내용을 넣으면 안된다는건 당연하고, 광고에 대해서는 금지하지는 않지만 과하게 상업적인 것이 확인되면 삭제할 수 있는 듯하다. 어차피 나는 상관없지만.
    > ......_귀하가 필요에 따라 귀하의 계정에 후원자의 이름이나 로고를 게시하여 귀하의 콘텐츠를 홍보할 수 있음을 이해하지만 귀하의 계정으로 게시되거나 귀하의 계정을 통해 서비스에 게시되는 콘텐츠가 광고 또는 홍보 마케팅에 중점을 두어서는 안 됩니다. 이는 페이지, 패키지, 리포지토리와 그 외 서비스의 모든 부분에 직접 게시되거나 이를 통해 게시된 콘텐츠를 포함합니다.  
    귀하의 계정과 연결된 README 문서 또는 프로젝트 설명 섹션에 정적 이미지, 링크, 홍보 텍스트 등을 포함할 수 있지만 이는 GitHub에서 호스팅하는 프로젝트와 관련이 있어야 합니다. 수익이 발생한 콘텐츠 또는 문제의 과도한 대량 콘텐츠를 게시하는 등 다른 사용자의 계정에서 광고를 할 수 없습니다.
    귀하는 서비스 약관 또는 사용 제한 정책에서 과도한 자동 대량 활동(예: 스팸), 일확천금 계획, 프로모션과 관련된 허위 진술 또는 기만을 포함해 불법에 해당하거나 그렇지 않으면 금지되는 콘텐츠 또는 활동을 홍보하거나 배포할 수 없습니다.  
    귀하의 계정에 프로모션 자료를 게시하기로 결정할 경우, 귀하는 미국 연방 거래위원회(FTC)의 보증 및 평가 지침을 포함하되 이에 국한되지 않는 모든 관련 법률 및 규정을 준수할 전적인 책임이 있습니다. 당사는 단독 재량에 따라 GitHub 약관 또는 정책을 위반하는 프로모션 자료 또는 광고를 제거할 권리가 있습니다._  
    출처: [Github에서의 광고](https://docs.github.com/ko/site-policy/acceptable-use-policies/github-acceptable-use-policies#10-advertising-on-github)

    > GitHub Pages is not intended for or allowed to be used as a free web-hosting service to run your online business, e-commerce site, or any other website that is primarily directed at either facilitating commercial transactions or providing commercial software as a service (SaaS). GitHub Pages 사이트는 암호 또는 신용 카드 번호 전송과 같은 중요한 트랜잭션에 사용하면 안 됩니다.
    [Github pages page limits](https://docs.github.com/ko/pages/getting-started-with-github-pages/github-pages-limits)

2. **용량 제한**
   - 소스 저장소는 1GB 제한을 추천. 100MiB 이상의 개별 파일은 업로드 불가
   - 배포되는 사이트 용량은 1GB미만 이어야 함
   - 빌드 및 배포에 10분이상 걸리면 timeout
   - soft bandwidth limit of 100GB/month
   - soft limit of 10 build/hour (일상적 사용에 문제 없음)
   - 트래픽 몰리면 Github에서 연락가고 사이트 다운될 수 있음.
참고로 1GB면 20KB 마크다운 파일 기준으로 5만개다. 결코 적은 수치는 아니라는 것을 알 수 있다. (단, 이미지, 동영상등은 직접 repo에 올리지 말고 다른 곳에 올린뒤 링크를 임베드 하는 방식으로 파일에 첨부할 것)

## 총평

생각보다 알아야 할 게 많았고... 그리고 생각보다 재미있기도 했다. 문서가 잘 되어 있기도 하고 오픈 소스라서 그런지 요모조모 뜯어보는 재미가 있달까... 근데 css 파일은 도저히 못 건드리겠다. 왤케 복잡함??? 네이버가 대기업인데에는 이유가 있다...