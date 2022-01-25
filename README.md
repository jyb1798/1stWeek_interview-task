# 잘하조 면접준비 과제(경험을 정리하는 팀과제)
 
저희 팀이 React 프로젝트 구조를 설계하고, CSS 방법론을 정하며 git commit 컨벤션을 결정하기 위해 조사한 내용을 정리하였습니다.
<details>
<summary>  <h2>[CRA Project Structure]</h2></summary>

# [1] CRA Project Structure

## RCA(react-create-app) 혹은 React 프로젝트 구조를 설계하는 방법론

### 결론: 정답은 없습니다.<br/>

<p><h4>하지만, 오답은 있습니다.</h4>
좋지 않은 설계의 특징을 공유하지 않는다면 프로젝트별의 특성과 구성원의 선호에 따라 자유롭게 구조를 설계하면 되는 겁니다.</p>

<p>React 공식 문서의 <a href=https://en.reactjs.org/docs/faq-structure.html>File Stucture</a>를 인용하면, React에서 범용적인 폴더 분류법을 지정해 추천하지는 않는다는 걸 알 수 있습니다. 그 대신에 일반적으로 인기 있는 접근법들을 설명해주고 있습니다.</p>

<p>따라서 저희 조는 참고할만한 몇 가지 출처에서 공통으로 나타나는 방법론들을 간단히 조사하고,<br/>여러 방법론 중 저희 팀원의 규모와 앞으로 다루게 될 프로젝트의 규모에 가장 알맞은 방법을 채택하기로 하였습니다.<br/>아래에 대표적인 피해야 할 특징들을 간단히 인용하겠습니다.</p></br>

### 추천되지 않는 방법들

<ul>
<li>너무 많은 <b>수직적</b> 계층구조
    <p>프로젝트의 규모가 커지면서, 모든 리액트 컴포넌트는 결국 복잡해집니다.<br/>
    더 많은 조건부 렌더링, 훅, 라우팅, 스타일, 테스트 등 다양한 기술적 고려사항 때문입니다.<br/>너무 깊은 계층구조는 인용, 유지보수, 신규 팀원의 프로젝트에 대한 이해에 대한 부분에 악영향을 끼치게 됩니다.<br/>따라서, 가능한 수직적 계층의 확장보다는 수평적인 계층의 확장을 고려해 보는 것이 좋습니다.</p>
    <pre>- src/
--- App/
----- index.js
----- test.js
----- style.css(혹은 js)
--- List/
----- index.js
----- test.js
----- style.css
----- hooks.js</pre>
위와 같은 상황에서 List 컴포넌트 안의 ListItem을 별도의 파일로 생성하기로 하면,
<pre>--- App/
----- index.js
----- test.js
----- style.css
--- List/
----- index.js
----- test.js
----- style.css
----- ListItem/
------- test.js
------- style.css</pre>
이러한 폴더 구조를 가지게 됩니다. 일반적으로 2계층의 구조까지는 문제가 없다고 생각되지만, 3계층 이상의 수직적인 구조를 가지는 건 추천되지 않습니다. <br/> 따라서 ListItem/ 하위의 다른 중첩된 폴더는 생성되지 말아야 합니다.<br/>
예시로 ListItem 안에서 Item마다 동적으로 생성되는 버튼을 다루는 컴포넌트인 ListButtons의 추가적인 확장이 필요할 때는, List 컴포넌트를 Content가 담길 placeholder 역할을 하는 Container 컴포넌트와, 안에 들어갈 내용을 담당하게 되는 ListContent로 분리할 수 있어 보입니다.
<pre>--- App/
----- index.js
----- test.js
----- style.js
--- Container/
----- index.js
----- test.js
----- style.js
--- ListConten/
----- index.js
----- test.js
----- style.js
----- ListButtons/
------- index.js
------- test.js
------- style.js
</li>
<br/>
<li>자주 함께 변경되는 파일들을 <b>수평적</b>으로 멀리 떨어트리지 마세요.
    <p>서로 긴밀한 연결 관계를 가져 동작해, 자주 묶어서 수정되는 파일들은 같은 수직적 계층 안에 보관되는 것이 직관적입니다.<br/>이러한 관계의 파일들을 멀리 떨어트려 놓는 건 프로젝트 구조에 대한 이해를 방해하는 요인이 될 수 있습니다</p>
</li>
</ul>
<br/>

### 앞으로 기본적으로 사용될 규칙들

- 파일들은 기본적으로 <b>기능</b>에 의해 분류됩니다.
  - 위에서 설명한 두 가지 규칙을 위배하지 않는 선에서, 파일들은 기능에 의해 묶입니다.
  - 이때 말하는 <b>'기능'</b>의 단위는 서비스 사용자의 UX를 구성하는 UI의 공통으로 묶일 수 있는 특징들을 의미하게 됩니다.
- 기능에 의해 분류되기 때문에, 라우팅을 사용할 경우 페이지별로 구분되게 됩니다.
  - UX를 기준으로 분류되기 때문에, UX의 많은 부분이 교체되는 페이지의 라우팅은, UI의 대분류로 기능하기에 알맞습니다.
- 특별하게 연관성을 가지지 않고, 반복성을 가지고 호출되는 컴포넌트만 component 폴더에 정리되게 됩니다
  - 앞으로 다루게 될 작은~중간 규모의 리엑트 프로젝트에서는 잘 보이지 않는 상황입니다.

<br/>

출처

<ul>
    <li><a href=https://en.reactjs.org/docs/faq-structure.html>File Structure</a></li>
    <li><a href=https://smoh.tistory.com/385>5단계로 보는 리액트 폴더구조</a></li>
</ul>
</br>
</br>
</details>
 

<details>
<summary><h2>[CSS 작성 방법]</h2></summary>

 # [2] CSS 작성 방법

우리팀은 앞으로의 프로젝트에 있어서 CSS 작성 방법을 토론했고, CSS의 발전 과정과 각 단계의 장,단점을 살펴본 뒤 CSS iN JS 방식인 <b>`Styled Component`</b>를 선택하였습니다. CSS가 왜 발전하게 되었는지, 그 문제점이 무엇이었는지에 대해 중점을 뒀습니다.

## CSS의 발전 순서

CSS의 발전 순서는 다음과 같습니다.
각 단계마다 전 단계의 문제점을 개선하기 위해 다음 단계의 CSS가 출시되었습니다.

1. CSS
2. CSS in CSS
   - CSS Pre-processor(SASS)
   - CSS Module
3. CSS in JS(Styled-Component)

## 1. CSS

### CSS의 단점

다음은 페이스북 개발자인 Christopher Chedeau aka Vjeux 가 소개한 CSS의 문제점입니다.

- Global namespace

  모든 스타일이 global에 선언되어 중복되지 않는 class 이름을 적용해야 하는 문제

  css의 가장 대표적인 문제점으로 어디에 선언을 하던지, 어디에 import를 하던지 항상 `global namespace`를 가집니다.

- Dead Code Elimination

  코드를 수정하고 삭제하는 과정에서 불필요한 CSS를 제거하기 어려운 문제

  CSS안에서 전혀 사용하지 않는 CSS가 있더라도 빌드 과정에서는 해당 CSS 라인은 여전히 남아있습니다. 또, 전임자(타인)이 작업했던 코드를 받게되면 어느 CSS를 사용하고 있고 어느 CSS가 불필요한지 확인하기가 어렵습니다.

- Minification

  클래스 이름의 최소화 문제

  CSS의 의도는 최소한의 코드로 반복적인 서식을 피하기 위함이었으나 복잡한 디자인과 레이아웃이 만들어지게 되면서 많은 양의 코드를 작성하게 되었습니다.

- Non-deterministic Resolution

  CSS 로드 순서에 따라 스타일 우선 순위가 달라지는 문제

  CSS의 우선순위 문제는 상당히 복잡합니다. CSS 셀렉터마다 고유한 Level과 포인트가 존재하고 그 총 합을 통해 우선순위가 결정되며 같은 우선순위라면 CSS가 나중에 적용되는 순서대로 덮어쓰도록 되어 있습니다.

- Isolation

  전역적인 속성으로 인해 CSS 에서는 파일이나 구성 요소 간에 격리를 하는 것은 사실상 불가능에 가깝습니다. 다만 인라인 스타일을 적용한다면 해결은 할 수 있습니다.

- Sharing Constants

  JS 코드와 상태값을 공유할 수 없는 문제

  과거에는 문제가 되었으나 `CSS Variable`이 해결책으로 등장하였고 CSS 기본기능으로 탑재 되었습니다.(IE11 제외)

- Dependencies

  CSS의 종속성 문제

  아무리 특정 요소가 특정 CSS에 의존하고 있다고 해도 스타일은 전역적이기 때문에 이를 명확하게 말하는 것은 힘들며, 모든 파일에서 모든 스타일 요소를 적용할 수가 있으므로 제어를 잃기가 쉽습니다.

## 2. CSS in CSS

이로 인해 CSS 전처리기가 등장합니다. 반복 작업 등 기존 CSS의 불리한 점을 해결하기 위해 개발되었습니다.

### 2-1 CSS Preprocessor(SASS)

- 개념</br>
  Syntax에 맞게 코드를 작성하고 컴파일러를 통해서 css 파일로 결과물이 나오는 구조입니다. 반복적인 부분을 스크립트나 변수로 처리할 수 있고, 대표적인 css 전처리기로는 `SASS`가 있습니다.

- 특징

  - 재사용성: 공통 요소 또는 반복적인 항목을 변수 또는 함수로 대체
  - 시간적 비용 감소: 임의 함수 및 Built-in 함수(여기서는 darken)로 인해 개발 시간적 비용 절약
  - 유지 관리: 중첩,상속과 같은 요소로 인해 구조화된 코드로 유지 및 관리가 용이

- 한계</br>
  전처리기를 위한 도구가 필요하며, 다시 컴파일하는 시간이 느릴 수 있습니다.

### 2-2 CSS Module

- 개념</br>

  - SASS는 클래스네임이 겹치지 않도록 개발자가 관리해야 하는 불편함이 있었습니다. 이로 인해서 CSS Module이 등장합니다.
  - CSS 모듈은 CSS를 모듈화 하여 사용하는 방식으로, 자동으로 고유한 클래스네임을 만들어서 scope를 지역적(local)으로 제한합니다. 때문에 css 파일이름(nameSpace)만 다르면 중복된 클래스네임을 사용할 수 도 있습니다.

- 특징

  - 클래스네이밍: 각각 CSS 파일 별로 각각의 CSS파일이 별도로 컴파일 되기 때문에 간단하게 네이밍된 클래스 선택자를 사용할 수 있고, 각각 중복된 이름이 가능하다. 실제 사용되는 클래스명은 자동으로 생성되고 유니크함이 보장된다.

- 한계</br>
  한 곳에서 모든 것을 작성하지 않기 때문에 별도로 많은 CSS 파일을 만들어 관리해야 합니다.

## 3. CSS in JS(Styled-Component)

CSS Module은 방대하고 많은 CSS 파일을 관리해야 하는 불편함이 있었습니다. 이로 인해서 CSS in JS가 등장합니다. 하나의 컴포넌트에 CSS를 같이 작성하며 이로인해 컴포넌트에 대한 관리를 쉽게 할수 있게 됩니다.

2014년 CSS의 문제점을 제기한 Vjuex가 처음 소개하였으며, 앞서 말한 문제점을 모든 이슈를 해결할 수 있다고 강조하였습니다.

### 특징

- props를 활용한 조건부 스타일링</br>
  JS와 CSS사이의 상수와 함수,변수를 하여, 이를 활용한 스타일링이 가능하다.

- 코드 경량화</br>
  짧은 길이의 유니크한 클래스를 자동으로 생성해준다.

### 한계

- FOUC(Flash of unsttyled content): 브라우저에 보여줄 것들이 많아짐에 다라, 점차적으로 속도가 느려진다. 특히, 컴포넌트가 렌더링되며 형태가 잡히기 때문에 원형(날것)의 모습이 잠깐 노출되는 현상인 FOUC가 나타난다.
- 인터랙션한 페이지일 경우 스타일이 동적인 변수에 의존하게 되어, 해당 컴포넌트가 리렌더링되면 CSS 파일을 따로 관리하는 방법에 비해 느린 성능을 보여줄 수 있다.
- 별도의 라이브러리를 설치해 하므로 번들의 크기가 커진다.

</br>

## 결론

- 큰 규모의 프로젝트가 아니기 때문에, 위에서 언급한 한계(성능 이슈, 번들 크기)는 해당되지 않는다고 판단하였습니다.
- CSS in JS로 작성한 코드를 CSS파일로 따로 최적화하여 추출해주는 라이브러리([Linaria](https://github.com/callstack/linaria),[Compiled](https://compiledcssinjs.com/))를 사용하면 성능 이슈를 해결할 수 있을 것이라 판단하였습니다.
- 지금은 `styled-component`로 결정하였지만, 필요시 다른 css(`sass`,`css-module`)도 유도리있게 사용하자고 결정하였습니다.

</br>
</br>
</details>
<details>
<summary>
<h2>Commit Message</h2></summary>
  # [3] Commit Message

## 깃 커밋 메세지 및 컨벤션 정리

### 커밋 타입

- **Feat :** 새로운 기능 추가 (a new feature)
- **Fix :** 버그 수정 (a bug fix)
- **Docs :** 문서 수정 (changes to documentation), ex) README.md 등
- **Style :** 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우 (formatting, missing semi colons, etc; no code change)
- **Refactor :** 코드 리펙토링 (refactoring production code) ex) (멘토 리뷰 반영 / 스스로 리팩토링 / 중복 코드 제거 / 불필요 코드 제거 / 성능 개선)
- **Test :** 테스트 코드, 리펙토링 테스트 코드 추가 (adding tests, refactoring test; no production code change)
- **Chore :** 빌드 업무 수정, 패키지 매니저 수정 (updating build tasks, package manager configs, etc; no production code change)
- **Add** - 레이아웃 / 기능 추가
- **Remove** - 내용 삭제 (폴더 / 파일 삭제)
- **Modify** - 수정 (JSON 데이터 포맷 변경 / 버튼 색깔 변경 / 폰트 변경)
  </br>

### 우리 팀 컨벤션 작성 예시

- git commit -m “Add : pages 폴더의 WireBarley.js 파일 레이아웃 추가”
- git commit -m “Modify : pages 폴더의 RedBrick.js 파일 레이아웃 변경”

출처

<ul>
    <li><a href=https://udacity.github.io/git-styleguide/>Udacity Git Commit Message Style Guide</a></li>
    <li><a href=https://webruden.tistory.com/486>베이스캠프 : 깃 커밋 메시지 컨벤션</a></li>
</ul>
</details>
