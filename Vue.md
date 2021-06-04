# Vue문법
### Vue에서 부모와 자식 컴포넌트 관계
* 위에서 아래로는 데이터(props)를 내리고
* 아래에서 위로는 이벤트를 올린다(event emit)
![image](https://user-images.githubusercontent.com/46732816/120740457-34352e00-c52e-11eb-84ea-83445accc3fa.png)

#### Props
* 프롭스는 상위 컴포넌트에서 하위 컴포넌트로 내리는 데이터 속성을 의미한다.
* 사용하는 이유는 모든 컴포넌트는 각 컴포넌트 자체의 스코프를 갖고 있어 다른 컴포넌트의 값을 바로 참조할수 없다.
```
<!-- 상위 컴포넌트 -->
<div id="app">
  <!-- 하위 컴포넌트에 상위 컴포넌트가 갖고 있는 message를 전달함 -->
  <child-component v-bind:propsdata="message"></child-component>
</div>
```
```
/ 하위 컴포넌트
Vue.component("child-component", {
  // 상위 컴포넌트의 data 속성인 message를 propsdata라는 속성으로 넘겨받음
  props: ["propsdata"],
  template: '<p>{{ propsdata }}</p>'
});

// 상위 컴포넌트
var app = new Vue({
  el: "#app",
  data: {
    message: "Hello Vue! from Parent Component"
  }
});
```
#### 같은 레벨의 컴포넌트 간 통신
* 동일한 상위 컴포넌트를 가진 하위 컴포넌트들 간의 통신은 아래와 같이 해야 한다.
  * Child(하위) -> Parent(상위) -> Children(하위 2개)
#### Event Bus
* 상위 - 하위 관계가 아닌 컴포넌트 간의 통신을 위해 Event Bus를 활용
```
 * Event Bus를 사용하기 위해 새로운 뷰 인스턴스를 생성
// 화면 개발을 위한 인스턴스와 다른 별도의 인스턴스를 생성하여 활용
var eventBus = new Vue();
new Vue({
  // ...
});
```
```
* 이벤트를 발생시킬 컴포넌트에서 $emit()호출
eventBus.$emit("refresh", 10);
```
```
* 이벤트를 받을 컴포넌트에서 $on() 이벤트 수신
// 이벤트 버스 이벤트는 일반적으로 라이프 사이클 함수에서 수신
new Vue({
  created: function() {
    eventBus.$on("refresh", function(data) {
      console.log(data); // 10
    });
  }
});
```
```
* 만약, EventBus의 콜백 함수 안에서 해당 컴포넌트의 메서드를 참고하려면 vm 사용
new Vue({
  methods: {
    callAnyMethod() {
      // ...
    }
  },
  created() {
    var vm = this;
    eventBus.$on("refresh", function(data) {
      console.log(this); // 여기서의 this는 이벤트 버스용 인스턴스를 가리킴
      vm.callAnyMethod(); // vm은 현재 인스턴스를 가리킴
    });
  }
});
```
#### 필터 사용법
* 텍스트형식화를 위해 사용 
  * ``` ex)<div v-bind:id="rawId | formatId"></div>``` 의 형태로 사용

#### 1. 전역필터 정의 법 
``` 
Vue.filter('makeComma', val => {
	return String(val).replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}); 
``` 

#### 2. 컴포넌트 옵션에서 로컬필터 정의
```
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
```
## Vue 비동기통신 
### Axios
* GET : 입력한 url에 존재하는 자원에 요청 
* POST : 새로운 리소스를 생성(create)할 때 사용
* Delete : REST 기반 API 프로그램에서 데이터베이스에 저장되어 있는 내용을 삭제하는 목적으로 사용합니다.
* PUT : REST 기반 API 프로그램에서 데이터베이스에 저장되어 있는 내용을 갱신하는 목적으로 사용됩니다.
### Async Await 
* 사용이유 : axios 모듈에서의 request / response는 비동기로 처리되기 때문에 어떤 처리 순서를 지정하지 않으면 request로 요청을 보내고 나서 response로 응답도 받기 전에 다음 구문을 수행
  * async 함수는 함수의 앞에 async를 붙여주고 함수의 내부 로직 중 비동기 처리 로직 앞에 await를 붙여준다.
  * 즉, Promise 객체를 반환하는 API 호출 함수 앞에 await를 붙인다. 
```
* 컴포넌트안 호출함수
async function fetchData() {
  await getUserList();
}
```
```

async testFunction() {
	const response = await axios.get('api/test')
	console.log(response)
	const response2 = await axios.get('api/test')
	console.log(response2)	
}
```
### Async /await 안의 반복문 사용 
![image](https://user-images.githubusercontent.com/46732816/120752909-5edeb100-c545-11eb-8127-626e1dd87bc3.png)

* await 안의 일반적인 반복문 사용시 해당이미지 처럼 올바른 순서로 반복을 수행하지않음
* 이러한 현상을 막기위해 **reduce** 사용
```
testFunction() {
	const data = [1,2,3,4,5]

	data.reduce((previous, current) => {
		return previous.then(async () => {
			await axios.get('api/test', current)
			console.log(response)
		})
	}, Promise.resolve())
}
```

# Vuex 문법 
### Vuex의 전체 흐름도
![image](https://user-images.githubusercontent.com/46732816/120738346-ae63b380-c52a-11eb-9a33-f8ee8a1e0434.png)

#### Vuex는 state, mutations, action, getters 4가지 형태로 관리가 된다.
* **State** : State는 Vue 컴포넌트에서 data로 볼 수 있다.  원본 소스의 역할을 하며, View와 직접적으로 연결되어있는 Model이다.
  * state는 직접적인 변경은 불가능하고 mutation을 통해서만 변경이 가능하다. 
  * mutation을 통해 state가 변경이 일어나면 반응적으로 View가 업데이트된다.
``` 
<! --state에 등록한 counter 속성  컴포넌트 템플릿 코드에서 접근방법 -->
$ this.store.state.counter 
```
* **Mutations** : Mutation은 state를 변경하는 유일한 방법이고 이벤트와 유사하다. 
  * mutation은 **함수로 구현**되며 첫 번째 인자는 state를 받을 수 있으며, 두 번째 인자는 payload를 받을 수 있다.
  * payload는 여러 필드를 포함할 수 있는 **객체형태도 가능**하다. 
  * 보통 API를통해 전달받은 데이터를 가공하여 State를 설정하는데 주로 사용한다.
 ``` 
  ex) store.commit('setData', payload) 
 ```
 ```
* Mutations 등록 

<!--  Vuex 에 mutations 속성을 추가한다. -->
export const store = new Vuex.Store({
  // ...
  mutations: {
    addCounter: function (state, payload) {
      return state.counter++;
    }
  }
});
 ```
``` 
 * Mutations 사용
 
 <!-- 컴포넌트에서의 사용 -->
 <div id="app">
  Parent counter : {{ parentCounter }} <br>
  <button @click="addCounter">+</button>
```
```
  <!-- commit을 통해 호출한다 -->
 methods: {
  addCounter() {
    // this.$store.state.counter++;
    this.$store.commit('addCounter');
  }
},
</div>
```
```
* Mutations에 인자 값 넘기기

 this.$store.commit('addCounter', 10);
this.$store.commit('addCounter', {
  value: 10,
  arr: ["a", "b", "c"]
});
```
* **Actions** : Action은 mutation과 비슷하지만 mutation과는 달리 **비동기 작업이 가능**하다. 
  * mutation에 대한 commit이 가능하여 action에서도 mutation을 통해 state를 변경할 수 있다.
  * action에서는 첫 번째 인자를 context 인자로 받을 수 있으며 이 context에는 **state, commit, dispatch, rootstate**와 같은 속성들을 포함한다.
  * setTimeout() 이나 서버와의 http 통신 처리 같이 결과를 받아올 타이밍이 예측되지 않은 로직은 Actions 에 선언한다.
  * *dispatch : 비동기처리 로직을 위한 것으로 Actions 를 이용할때 commit을 사용한것처럼 dispatch를 사용한다.
```
this.$store.dispatch('addCounter');
```
```
* Actions 에 인자 값 넘기기

<!-- by 와 duration 등의 여러 인자 값을 넘길 경우, 객체안에 key - value 형태로 여러 값을 넘길 수 있다 -->
<button @click="asyncIncrement({ by: 50, duration: 500 })">Increment</button>
```
```
export const store = new Vuex.Store({
  actions: {
    // payload 는 일반적으로 사용하는 인자 명
    asyncIncrement: function (context, payload) {
      return setTimeout(function () {
        context.commit('increment', payload.by);
      }, payload.duration);
    }
  }
})

```


