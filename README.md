# vue2-basiic-cdn-zerocho

<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">CDN</summary>
<br>

Webpack Babel이 아닌 순수 HTML로 Vue를 구현한다.

```html
<!DOCTYPE html>
<head>
  <title>구구단</title>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"><!-- 태그 추가 --></script>
</head>
<body>
</body>
</html>
```


</details>
<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">Vue 인스턴스 초기화 및 root 영역 할당</summary>
<br>

```html
<body>
  <div id="root">
  </div>
</body>
<script>
  /* CDN Script를 통해 아래 코드만으로 Main div를 Vue의 컴포넌트, 템플릿으로 전환할 수 있다. */
  const app = new Vue({
    el: '#root' /* HTML상의 해당 영역에 Vue 인스턴스가 적용된다. */
  });
</script>
```


</details>


<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">data 속성</summary>
<br>

Vue나 React 같은 싱글페이지 애플리케이션의 경우 브라우저의 주소창이 변경되지 않는다.
즉, 하나의 페이지에서 화면을 바꾸는 원리이므로 기존 코딩된 영역 혹은 데이터가 다른 영역/데이터로 변경된다.
따라서 뷰나 리액트에서는 어떤 영역이 변경 되는지를 파악하는것이 중요하고,
변경되는 대상을 데이터로 관리해야 한다.

예를들어 좋아요 버튼을 눌렀을때 버튼이 사라지면서 "좋아요 버튼 눌림" 이라는 텍스트가 출력되기 위해서는
좋아요 버튼이 데이터로서 관리되어야 한다.

```html
<script>
  const app = new Vue({
    el: '#root',
    data: {
      liked: false
    }
  });
</script>
```
기존 바닐라 자바스크립트나 jQuery의 경우 데이터가 바뀌면 변경된 데이터에 따라서 append remove 등의 작업을 따로 코드를 통해 dom을 수정을 해줘야하지만 
Vue에서는 데이터의 변경을 감지하고 데이터가 사용되는 영역을 자동으로 수정해주기 때문에 작업이 줄어든다.

**`Vue는 데이터만(를) 관리한다`를 핵심 으로 생각, 사고방식을 바꿔야만 퍼블리싱, JQuery를 했던 사람들이 Vue나 React에 적응하기 쉬워진다.**
해당 데이터는 위의 코드와 같이 data 속성을 정의하여 객체로 관리한다.
해당 객체는 this로 접근이 가능하다.
`data.liked` == `this.liked`

</details>
<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">methods 속성</summary>
<br>

Vue 에서 methods 속성은 반응형 데이터인 data를 사용해 특정 동작을 수행하는 함수를 정의하는데 사용한다.
methods에 정의된 함수는 UI의 이벤트(클릭, 입력 등)에 반응하여 데이터의 상태를 변경하거나 필요한 작업을 수행한다.

일반적으로 methods 속성을 사용하는 이유는 다음과 같다.
 - 이벤트 기반 동작: 버튼을 클릭하거나 특정 액션이 발생했을 때 동작할 함수를 정의할 수 있다.
 - 데이터 변경, 화면 업데이트: methods 속성 내 함수에서 data 속성의 값을 변경하면, Vue의 반응형 시스템이 해당 데이터와 연결된 화면 영역을 자동으로 업데이트 한다.

 ```html
<script>
  const app = new Vue({
    el: '#root',
    data: {
      liked: false
    },
    methods: {
      onClickButton() {
        this.liked = true; // data.liked = true와 같게 동작된다.
      }
    }
  });
</script>
```
위 코드에서 onClickButton 메소드는 liked의 상태를 true로 변경하여 최초 1회 false를 true로 변경하게 한다.  
이처럼 Vue에서는 이벤트에 따라 동작을 분리하여 정의하고 데이터를 제어하는 방식으로 개발하는 것이 중요하다.
기존 바닐라 자바스크립트나 jQuery의 경우 클릭시 dom을 수동으로 조작하고, 버튼 상태에 따라 새로운 HTML을 추가하거나 CSS를 수정해야 했다.  
반면 Vue는 methods 속성에서 데이터를 변경하는 것만으로 Vue의 반응형 시슽메이 DOM을 자동으로 조작해주기 때문에 코드의 가독성과 유지보수가 크게 향상된다.
**핵심적으로 Vue에서는 데이터와 동작을 분리하여, 필요한 동작은 methods에 정의하고, UI는 데이터에 따라 자동으로 업데이트된다고 이해하면 좋다**

</details>
<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">v-on 디렉티브</summary>
<br>

js에서는 이벤트 리스너에 함수 등록을 onclick이라는 속성에 등록한다.  
vue에서는 v-on 디렉티브를 활용한다.  
`v-on:이벤트명="함수명"` 형태로 특정 이벤트에 통해 함수를 바인딩한다.  
아래 예제는 click이벤트에 onCLickButton을 `v-on:click="onClickButton"` 형태로 바인딩하는 예제 코드이다.  

- ### 예제코드

  ```html
  <!DOCTYPE html>
  <html lang="ko">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>좋아요</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      <button v-on:click="onClickButton">Like</button> <!-- v-on 디렉티브 이벤트 바인딩 -->
    </div>
  </body>
  <script>
    const app = new Vue({
      el: '#root',
      data: {
        liked: false,
      },
      methods: {
        onClickButton() {
          console.log(this.liked)
          this.liked = true;
          console.log(this.liked)
        }
      }
    });
  </script>
  </html>
  ```

</details>
<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">v-if 디렉티브 (v-else-if/v-else)</summary>
<br>

# `v-if`
js에서는 조건부 렌더링을 dom 객체를 직접 탐색하여 css 속성을 부여하여 제어한다.  
react에서는 3항연산자나 && 혹은 || 조건으로 데이터 혹은 JSX 태그를 직접 제어한다.  
vue 에서는 v-if 디렉티브 속성을 data 변수와 함께 부여하여 조건부 렌더링을 적용한다.

- ### 예제코드

  ```html
  <!DOCTYPE html>
  <html lang="ko">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>좋아요</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="root">
      <div v-if="liked">좋아요 눌렀음</div> <!-- v-if 조건부 랜더링 적용 -->
      <button v-on:click="onClickButton">Like</button>
    </div>
  </body>
  <script>
    const app = new Vue({
      el: '#root',
      data: {
        liked: false,
      },
      methods: {
        onClickButton() {
          console.log(this.liked)
          this.liked = true;
          console.log(this.liked)
        }
      }
    });
  </script>
  </html>
  ```

# `v-else ~ v-else-if`

 - v-else-if : v-if조건에 부합하지 않으면서 새로운 조건을 지정한다.  
 - v-else : 모든 조건에 부합하지 않는 경우에 해당한다.  
- ### 예제코드

  ```html

  <body>
    <div id="root">
      <div v-if="true">if</div>
      <div v-else-if="!true">else-if</div>
      <div v-else>else</div>
    </div>
  </body>

위와같이 v-if를 포함한 조건부 렌더링 디렉티브는 연속된 형제노드로 사용해야만 제대로 작동한다.
만약 조건부 렌더링 디렉티브 사이에 형제 노드로 일반 태그를 선언한다면, v-if와 v-else-if(혹은 v-else)를 연결하지 못해 조건부 렌더링이 적용되지 않고 일반 태그만 출력된다.
- ### 예제코드

  ```html

  <body>
    <div id="root">
      <div v-if="true">if</div>
      <div>NaN</div> <!-- 해당 영역만 출력됨. -->
      <div v-else>else</div>
    </div>
  </body>
  ```

</details>

<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">보간법(Mustache 구분)</summary>
<br>

보간법이란?  
템플릿 영역에서 태그 사이에 텍스트 노드를 data 변수 등으로 할당하는 문법이다.  
react를 예로 들면 일반적인 단일 중괄호 표현식을 사용하며 이를 보간법이라고 한다.

Mustache란?  
vue에서 사용하는 보간법 방식으로 `{{ 변수 혹은 값 }}` 과 같이 이중 중괄호 형태로 데이터를 바인딩시킨다.  

- ### 예제코드

  ```html
  <body>
    <div id="root">
      <div>{{ first }} + {{ second }}</div>
    </div>
    <script>
      const app = new Vue({
        el: '#root',
        data: {
          /* 구구단 예제 템플릿 상 값이 변경되는 부분 4곳 */
          first: Math.ceil(Math.random() * 9),
          second: Math.ceil(Math.random() * 9),
        },
        
      })
    </script>
  </body>
  ```
  위의 예제에서는 랜덤값이 보간법에 의해 할당되어 실제 렌더링시 브라우저에 값 + 값 형태로 출력된다.


</details>
<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">템플릿 참조 (ref / $refs)</summary>
<br>

Vue에서 기본 DOM 요소에 직접 액세스 해야하는 경우 ref 속성을 사용한다.  
- ### 예제코드

  ```html
  <input ref="input">
  ```

위 예제코드와 같이 일반 DOM요소에서 사용되는 경우는 해당 요소가 되지만, 자식 컴포넌트에서 사용하는 경우 참조는 해당 컴포넌트 인스턴스가 된다.
- ### 예제코드

  ```html
  <Child ref="child" />
  ```

위와같이 DOM요소에 ref속성을 적용한 뒤 접근할때에는 `this.$refs.ref명` 문법으로 접근한다.  


- ### 예제코드
  ```js
  onSubmitForm(e) {
    console.log(this.$refs.input)
    console.log(this.$refs.child)
  }
  ```

</details>
<details>
<summary style="font-size:30px; font-weight:bold; font-style:italic;">접은글 템플릿</summary>
<br>

- ### 예제코드

  ```html
  ```

- ### 예제코드
  ```js
  ```

</details>