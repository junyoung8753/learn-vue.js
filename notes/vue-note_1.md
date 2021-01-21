---
title: 'Vue.js 시작하기'
permalink: /vue/
type: pages
sidebar:
  nav: 'vueside'
toc: true
toc_label: 'Vue.js 시작하기'
---

<br>

# 섹션 1. Vue.js 소개

---

<br/>

## 1. MVVM 모델에서의 Vue

<br/>

<!-- vue1.png -->

![vue_1-1](/posting/vue_pages/notes_img/vue_1-1.png)

<br/>

---

<br/>

## 2. 기존 웹 개발 방식

<br/>

HTML, CSS, JavaScript 를 따로따로 작성해서 웹 구현

```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app"></div>

    <scrip>
      // console.log(div);
      var div = document.querySelector('#app');
      var str = 'hello world';
      div.innerHTML = str;
      str = 'hello world!!!';
      div.innerHTML = str;
    </scrip>
  </body>
</html>
```

<br/>

---

<br/>

## 3. Reactivity 구현 (반응성)

<br/>

`Reactivity` 란 데이터의 변화를 라이브러리에서 감지해서 알아서 화면을 자동으로 그려주는것.

```js
Object.defineProperty(대상 객체, '객체의 속성', {
            //정의할 내용

  //속성에 접근했을 때의 동작을 정의
  get:function(){
  },

  //속성의 값을 할당했을 때의 동작을 정의
  set: function(){
  },
})
```

Object.defineProperty()

<br>

```js
Object.defineProperty(viewModel, 'str', {
  get: function () {
    console.log('접근');
  },
  set: function (newValue) {
    console.log('할당', newValue);
    div.innerHTML = newValue;
  },
});
```

콘솔에서 viewModel.str을 치면  
'접근' 이 출력되고

콘솔에서 viewModel.str = "값" 을 바꾸면  
'할당', newValue(받은값) 이 출력되고  
div.innerHTML에 newValue 가 출력된다.
(데이터가 바인딩되어 브라우저창에 값이 출력되었다 )

(데이터 바인딩: 함수를 호출할때 그 해당 함수가 위치한 메모리 주소로 연결해주는것을 의미)

<br>

---

<br>

## 4. Reactivity 코드 라이브러리화 하기

<br>

코드

```js
(function () {
  function init() {
    Object.defineProperty(viewModel, 'str', {
      get: function () {
        console.log('접근');
      },
      set: function (newValue) {
        console.log('할당', newValue);
        render(newValue);
      },
    });
  }
  function render(value) {
    div.innerHTML = value;
  }
  init();
})();
```

<br>

구조

```javascript
{
  전체함수;
  {
    함수init;
    {
      속성정의함수;
      {
        함수get;
        console.log(출력);
      }
      {
        함수set(값);
        console.log(출력, 값);
        전달함수(값);
      }
    }
  }
  {
    전달함수(받은값);
    innerHTML = 받은값;
  }
  {
    init;
  }
}
```

<br>

진행순서:

```md
전체함수 -> 함수init -> 속성정의함수 ->  
함수get ->출력 ->  
함수set -> 출력 -> 안쪽전달함수 ->  
밖전달함수 -> innerHTML=받은값 출력
```

<br>

---

<br>

## 5. Hello Vue.js와 뷰 개발자 도구

<br>
Vue 스크립트 소스

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

<br>

---

---

<br/>

# 섹션 2. 인스턴스

---

## 1. 인스턴스 소개

<br/>

* 인스턴스는 뷰로 개발할 때 필수로 생성해야 하는 코드

* 모든 Vue 앱은 Vue 함수로 새 Vue 인스턴스를 만드는 것부터 시작한다.
<br>

_인스턴스 생성 방법_

1. body 안에 script src 와 script 태그를 붙여준다
2. `el` 에 HTML element를 쓴다
3. 그 외 속성과 api를 쓴다.

```html
<body>
  <div id="app"></div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        message: 'hi',
      },
    });
  </script>
</body>
```

<br>

---

<br/>

## 2. 인스턴스와 생성자 함수

<br/>

**생성자 함수** : _재사용할 수 있는 객체 생성 코드를 구현하는 것._

- 함수 이름의 첫 글자는 대문자로 시작합니다.

- 반드시 "new" 연산자를 붙여 실행합니다.

<br/>

크롬 콘솔창에서 생성자 함수를 만든 모습.

```js
> function Person(name, job){
  //this = {}; 빈 객체가 암시적으로 만들어짐
      this.name = name;
      this.job = job;
  //새로운 프로퍼티를 this에 추가함.
  }
  //this가 암시적으로 반환됨.
< undefined
  ---------------------------
> var p = new Person ('josh', 'developer')
  // p라는것을 Person 함수를 이용해 만들고 this의 속성안에 값을 넣어 정의해줌.
< undefined
  ---------------------------
> p
  //p를 실행

< Person {name: "josh", job: "developer"}
  job: "developer"
  name: "josh"
  __proto__: Object
  // p의 속성이 정의된것을 볼수있다.
  ---------------------------
```

<br/>

---

<br/>

## 3. 인스턴스 옵션 속성

<br/>

인스턴스의 속성, API들

```js
new Vue({
  el: ,
  template: ,
  data: ,
  methods: ,
  created: ,
  watch: ,
});
```

el : _인스턴스가 그려지는 화면의 시작점 (특정 HTML 태그)_

template : _화면에 표시할 요소 (HTML, CSS 등)_

data : _뷰의 반응성(Reactivity)이 반영된 데이터 속성_

methods : _화면의 동작과 이벤트 로직을 제어하는 메서드_

created : _뷰의 라이프 사이클과 관련된 속성_

watch : _data에서 정의한 속성이 변화했을 때 추가 동작을 수행할 수 있게 정의하는 속성_

<br>

---

---

<br/>

# 섹션 3. 컴포넌트

---

## 1. 컴포넌트 소개

<br/>

`컴포넌트`는 화면의 영역을 구분하여 개발할 수 있는 뷰의 기능이다.

컴포넌트 기반으로 화면을 개발하게 되면 코드의 __재사용성__ 이 올라가고 빠르게 화면을 제작할 수 있다.

![vue_3-1](/posting/vue_pages/notes_img/vue_3-1.png)

<br/>

---

<br/>

## 2. 컴포넌트 등록 및 실습

<br/>

```html
  <body>
    <div id="app">
      <app-header></app-header>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
  // Vue.component('컴포넌트 이름', 컴포넌트 내용);
      Vue.component('app-header', {
        template: '<h1>Header</h1>',
      });
// Vue 생성, ele 선택
      new Vue({
        el: '#app',
      });
    </script>
  </body>
```

1. new Vue를 만들어 el: '#app'을 잡는다.
2. Vue.component라는 메소드로 'app-header'란 이름을 준다
3. 컴포넌트 내용에 template속성을 이용해 \<h1>Header\</h1>를 넣었다.
__정리__: Vue 로 el #app을 선택후 컴포넌트로 이름과 속성을 주고 선택한 #app태그 안쪽에 이름'app-header'를 넣으면 속성이 실행된다.

<br/>

---

<br/>

## 3. 전역 컴포넌트 등록

<br/>

```html
  <body>
    <div id="app">
      <app-header></app-header>
      <app-content></app-content>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
  // (Vue.componet = 전역컴포넌트)
      Vue.component('app-header', {
        template: '<h1>Header</h1>',
      });

      Vue.component('app-content', {
        template: '<h2>content</h2>',
      });

      new Vue({
        el: '#app',
      });
    </script>
  </body>
```

<br/>

---

<br/>

  ## 3. 지역컴포넌트


```html
  <body>
    <div id="app">
      <app-header></app-header>
      <app-content></app-content>
      <app-footer></app-footer>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>

      // (Vue.componet = 전역컴포넌트)
      Vue.component('app-header', {
        template: '<h1>Header</h1>',
      });
      Vue.component('app-content', {
        template: '<h2>content</h2>',
      });

      new Vue({
        el: '#app',
        //지역 컴포넌트 등록방식
        components: {
          // '컴포넌트 이름(key)': {컴포넌트 내용(value)}
          'app-footer': {
            template: '<footer>footer</footer>',
          },
        },
      });
    </script>
  </body>
</html>
```

---

<br/>

## 4. 전역 컴포넌트와 지역 컴포넌트의 차이점

* 지역 컴포넌츠는 복수이기때문에 s가 붙어서 components라고 한다.  

* 일반적으로 지역 컴포넌츠로 등록해나가면서 사용을 하며 전역컴포넌트는 라이브러리나 전역으로 세팅할때 사용한다.

---

<br/>

## 5. 컴포넌트와 인스턴스와의 관계

<br>

지역컴포넌트를 등록안한 지역에 사용하면 작동하지 않으며 일반적으로 실무에서는 전역컴포넌트보다는 지역컴포넌트를 사용한다.  
즉, 인스턴스 하나를 붙이고 그안에 컴포넌트 붙여나가는 방식으로 진행한다.

---

---

<br/>

# 섹션 4. 컴포넌트 통신 방법 - 기본

---

## 1. 컴포넌트 통신

<br/>


![vue_4-1](/posting/vue_pages/notes_img/vue_4-1.png)

---

<br>

## 2. 컴포넌트 통신 규칙이 필요한 이유

<br>

데이터의 흐름을 추적하기 어려운 문제점을 해결하기 위하여 event 를 올려보내고 props를 내려보내는 방식의 규칙으로 사용한다.

![vue_4-2](/posting/vue_pages/notes_img/vue_4-2.png)

---

<br>

## 3. props 속성

- __props__: property의 약자로 부모에게 받아온 데이터.

<br>

```html
<app-header v-bind:프롭스 속성이름="상위 컴포넌트 데이터 이름"></app-header>
```

의 문법을 사용하여 components안에 가져오고싶은 data를 가져올수있다.

---

<br>

## 4. props 속성의 특징

정리:
1. Vue를 생성후 el: '#app' 지역에 사용하겟다고 말한다
2. components: 와 data: 를 만든다.
   * components의 이름을 app-header라짓고 안에 template과 props의 속성을 줫다.
   * data에는 message에 속성을 줫다.
3. 그후 app-header(컴포넌트)에 만들어놓은 data를 가져다 썻다. 
```html
    <app-header v-bind:propsdata="message"></app-header> 
```

```html
  <body>
    <div id="app">
      <!-- <app-header v-bind:프롭스 속성이름="상위 컴포넌트 데이터 이름"></app-header> -->
      <app-header v-bind:propsdata="message"></app-header>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      var appHeader = {
        template: '<h1>header<h1>',
        props: ['propsdata'],
      };

      new Vue({
        el: '#app',
        components: {
          'app-header': appHeader,

        },
        data: {
          message: 'hi',
        },
      });
    </script>
  </body>
```

---

<br>

## 5. props 속성 실습

```html
  <body>
    <div id="app">
      <app-header v-bind:propsdata1="message"></app-header>
      <app-content v-bind:propsdata2="num"></app-content>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>

      var appHeader = {
        template: '<h1>{{ propsdata1 }}</h1>',
        props: ['propsdata1'],
      };

      var appContent = {
        template: '<div>{{propsdata2}}</div>',
        props: ['propsdata2']
      };

      new Vue({
        el: '#app',

        components: {
          'app-header': appHeader,
          'app-content': appContent,
        },
        
        data: {
          message: 'hi',
          num: 10,
        },
      });

    </script>
  </body>
```

크롬 Vue창과 화면창에  
propsdata1: hi  
propsdata2: 10  
이 나온다.

---

<br>

## 6. event emit

<br>

* event 올라가는 구조: ([컴포넌트 통신 방식](#2.-컴포넌트-통신-규칙이-필요한-이유) 참조)

<br>

```html
  <body>
    <div id="app">
      <app-header> </app-header>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <script>
      var appHeader = {
        template: '<button v-on:click="passEvent">click me</button>',
        methods: {
          passEvent: function () {
            this.$emit('pass');
          },
        },
      };

      new Vue({
        el: '#app',
        components: {
          'app-header': appHeader,
        },
      });
    </script>
  </body>
```

1. methods에 passEvent를 정의했다.
2. template에 버튼을 만들고 v-on:click="____"을 준다음 passEvent 함수를 넣어줫다.
3. this.$emit() 라는 Vue.js에서 제공하는 api(기능)을 이용하여 'pass'라는 값을 넣어줫다.

---

<br>

## 7. event emit으로 콘솔 출력하기

<br>

![vue_4-7](/posting/vue_pages/notes_img/vue_4-7.png)

* 상위에서 하위로는 데이터를 내려줌, 프롭스 속성
* 하위에서 상위로는 이벤트를 올려줌, 이벤트 발생

---

<br>

## 8. event emit 실습

<br>

```html
 <body>
    <div id="app">
      <p>{{num}}</p>
      <app-header v-on:decrease="decreaseNumber"> </app-header>
      <app-content v-on:increase="increaseNumber"> </app-content>

    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
    //하위컴포넌트(appHeader)
      var appHeader = {
        template: '<button v-on:click="deleteNumber">minus</button>',
        methods: {
          deleteNumber: function () {
            this.$emit('decrease');
          },
        },
      };
    // 하위컴포넌트(appContent)
      var appContent = {
        template: '<button v-on:click="addNumber">plus</button>',
        methods: {
          addNumber: function () {
            this.$emit('increase');
          },
        },
      };
    //상위 컴포넌트(root)
      new Vue({
        el: '#app',
        components: {
          'app-header': appHeader,
          'app-content': appContent,
        },
        methods: {
          decreaseNumber: function () {
            this.num = this.num - 1;
          },
          increaseNumber: function () {
            this.num = this.num + 1;
          },
        },
        data: {
          num: 10,
        },
      });
    </script>
  </body>
  ```

  ---

## 9. 인스턴스에스의 this

<br>

this 는 해당 object를 가리키며 
this의 속성이름을 사용해서 해당위치나 값을 구한다.  

```js
var vm = new Vue({
  el: '#app',
  components: {
    'app-header': appHeader,
    'app-content': appContent,
  },
  methods: {
    decreaseNumber: function () {
      this.num = this.num - 1;
    },
    increaseNumber: function () {
      this.num = this.num + 1;
    },
  },
  data: {
    num: 10,
  },
});
```
__ex)__

* this는 vm을 가리킨다.  
* this.num = 10 이다
