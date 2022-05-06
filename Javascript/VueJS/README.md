Vue.js
====

# 컨벤션

- javascript와 vue 코드의 작성은 airbnb와 vue/essential 가이드를 따른다
  - [aribnb javascript guide](https://github.com/ParkSB/javascript-style-guide)
  - [vue/essential guide](https://vuejs.org/style-guide/rules-essential.html#use-detailed-prop-definitions)
- style영역은 scoped scss를 사용하고 클래스의 명명규칙은 BEM을 사용한다

# 구조

* 컴퍼넌트 구분
  - components: 어플리케이션 전역에서 사용되는 재사용 가능한 컴퍼넌트
  - modules: 특정 페이지내에서 사용되는 재사용 가능한 컴퍼넌트
  - pages: 라우터를 가지는 View 컴퍼넌트
  - layouts: 라우터의 레이아웃을 정의하는 컴퍼넌트

* Vue 기능관련
  - store: vuex를 사용하며 기능단위로 상태를 구분
  - router: 페이지의 진입경로를 관리
  - directives: 커스텀 directives
  - filters: 커스텀 filters

* lib
  - 외부 서비스의 SDK 또는 라이브러리를 다루는 영역

* assets
  - 페이지 내 리소스

* styles
  - 페이지 전역 스타일

* constants
  - 페이지에서 사용하는 함수
 
 ## Module Import

 - 모듈의 Import는 동일한 경로를 제외하고 절대경로를 사용합니다.
  ```
  @/components/foo

  ./foo
  ```

- js 파일이 아닌 경우 확장자를 표기하고 파일명을 그대로 사용합니다.
```
  @/components/foo.vue;  (O)
  @/components/foo.vue; (X)

  @import '@/scss/base/_colors.scss';  (O)
  @import '@/scss/base/colors.scss';  (X)
```

## template
* 태그의 명명과 프로퍼티바인딩은 스네이크케이스로 표기한다.
```html
(O)  <eight-percent/>
(X)  <EightPercent/>
``` 

* Slot영역이 필요없는 경우 축양형으로 표기한다.
```html
(O)  <eight-percent/>
(X)  <eight-percent></eight-percent>
```

* 프로퍼티와 어트리뷰트를 여러줄로 내려쓰는 경우 줄바꿈 이후 작성한다.
```html
(O)
<eight-percent
  v-if="loaded"
  :data-index="dealIndex"
/>

(X)
<eight-percent v-if="loaded"
  :data-index="dealIndex"
/>
```

* 단 순수한 HTML태그에는 많은 요소가 바인딩되지 않는다면 한줄선언을 허용한다.
```html
(O)
<div id="foo" class="bar"></div>
(-)
<div
  class="x"
></div>
```

* 프로퍼티와 어트리뷰트 나열은 기본적으로 다음의 순서로 진행한고 중요도에 따라 순서는 조정될 수 있다.
```html
<eight-percent
  // 정적프로퍼티
  class=""
  // 조건부 바인딩
  v-if="loaded"
  // 리스트랜더링
  v-for="(item, index) in items"
  // 어트리뷰트 바인딩
  :class="{ active: isActive }"
  :style="{ color: activeColor, fontSize: fontSize + 'px' }"
  // 이벤트 바인딩
  @click="event"
  // Props 바인딩 (Props는 Array 또는 primary type의 데이터를 사용한다)
  :data-index="dataIndex"
/>
```

* `v-if`와 `v-for`는 동시에 사용하지 않고, 조건부 리스트랜더링의 경우 computed속성에서 filter한다.
* style, class 바인딩은 문자열보다는 객체나 배열로 정의한다.
* 단 이벤트바인딩은 반드시 카멜케이스로 작성한다. (케밥케이스 작성 시 오류가 발생할 수 있음)

## Style

* 레이아웃, 믹스인, 변수 등 전역 스타일을 제외한 영역에서 scoped scss를 사용한다. `<style lang="scss" scoped>`
* 클래스의 명명은 BEM을 따른다.
* ID선택자는 반드시 페이지에서 유일함이 보장되어야 하는 테그에만 사용한다.
* Module속성은 거의 사용하지 않지만 필요할때 참조하여 사용한다.
```vue
// module css 예제
<script>
data() {
  return {
    color: this.$colors.black,
    redColor: this.$style.red,
  };
}
</script>

<style lang="scss" module="$colors">
@import '@/scss/base/_colors.scss';

:export {
  black: $color-gr-700;
}
</style>

<style module>
.red {
  color: red;
}
</style>
```

## Component

* 컴퍼넌트의 구성은 아래와 같은 항묵으로 구분된다.
  * layouts  : 전체적인 구조 구성을 위한 컴퍼넌트
  * pages    : 하나의 라우터를 가지는 페이지 단위 컴퍼넌트
  * modules  : page 단위에서 자체적으로 동작하는 모듈화된 컴퍼넌트
  * components : 어플리케이션 전체에서 사용되며 상태를 가질 수 없는 컴퍼넌트

* 컴퍼넌트는 기본적으로 파일명과 일치하나 구분이 필요한 경우 prefix를 붙여준다.
```javascript
components: {
  'el-title': Table,
},
```

* 컴퍼넌트의 구성은 다음의 순서를 따르고 사용되는 항목이 없는경우 작성하지 않는다
```javascript
import Foo from 'path';

export default {
  name: '',
  mixins: [],
  props: {},
  component: {},
  data() {
    return {

    }
  },
  computed: {},
  watch: {},
  methods: {},
  
  // LifeCycle Hooks
  created() {},
}

```

* Props는 단순나열이 아닌 구체적으로 정의하고 Optional값은 default값을 명시한다.
* validator는 필요에 따라 선택 적용한다.
* `required: true`는 없을때 문제가 되야하는 경우에만 사용한다.
```javascript
props: {
  props1: {
    type: String,
    required: true,
  },
  props2: {
    type: String,
    default: '',
  },
  props3: {
    type: Array,
    default: () => [],
  },
  props4: {
    type: Object,
    default: () => ({
      foo: '',
      bar: 1
    }),
  },
  props5: {
    type: String,
    validator: function (value) {
      // The value must match one of these strings
      return ['success', 'warning', 'danger'].indexOf(value) !== -1
    }
  },
}
```

* 가급적 단방향 데이터흐름을 유지한다.

# Store
* Store의 Vuex 기본문법을 사용하고 스토어의 의존성이 큰 경우 Helper함수를 사용한다.
```javascript
// Action
this.$store.dispatch('Action Name', value)

// Mutation
this.$store.commit('Mutation Name', value);

// Getter
this.$store.state['State Name']
```

* 스토어는 기능별 모듈화하여 사용 함
* 기본구성은 아래와 같으며 mutation-type은 선택적 사용
```javascript
import request from 'superagent';

export default {
  namespaced: true,

  state: {
    state1: '',
  },
  getters: {
    // get + 상태명을 주로 사용하고 의미있는 명사나 상태값과 동일하게 사용하기도 한다.
    getState1: '',
  },
  mutations: {
    // set + 상태명으로 사용하고 상태를 변이시키는 코드가 아닌 포함하지 않는다.
    setState(state, value) {
      state.dealNotice = value;
    },
  },

  actions: {
    // 비동기처리나 행위를 기술하고 네이밍의 제약은 없다.
    fetchData(context, id) {
      return new Promise((resolve, reject) => {})
    },
  },
};

```

* Store
  * Action은 비동기과정이나 유저의 행위를 정의하여 사용한다.
  * 컴퍼넌트에서 상태변화를 일으킬때는 mutation명으로 의미가 전달되지 않는다면 action을 통해서 접근하도록 한다.

* Helper는 편의에 따라 사용되며 다양한 형태로 사용된다.
```javascript
// helper함수 이용
import { createNamespacedHelpers } from 'vuex';
const { mapState, mapActions } = createNamespacedHelpers('[모듈네임]');

...mapActions([
  '[ActionName]',
]),

// 모듈과 이름을 같이 사용
import { mapState, mapActions } from 'vuex'
...mapActions([
  '[모듈네임]/[ActionName]',
]),


// 모듈별 구분 (하나의 컴퍼넌트에서 다양한 모듈 Store를 참조할 때)
import { mapState, mapActions } from 'vuex'
...mapActions('모듈네임', [
  'ActionName1',
  'ActionName2',
]),
```

## Router

1. 라우터 끝에는 '/'를 붙인다. (Django 규칙과 동일하게)
2. 라우터는 가급적 이름을 가지도록 하고 name으로 사용한다.
  ```javascript
    this.$router.push({ 
      name: '',
      params: {},
      query: {},
    });
  ```

## Watch 패턴
1. Watch는 다음과 같이 정의되어 사용한다.
```
watch: {
  watchDataName() {
  },
},
```
2. 복잡도가 높거나 watch option이 필요한 경우 다음의 형태로 사용한다.
```
methods: {
  todo1() { },
  todo2() { },
},
watch: {
  watchDataName: {
    deep: true, // option: object 또는 array를 감시할때 (default: false)
    immediate: true, // option: 페이지 로드 즉시 실행 (default: false)
    handler: [
      'todo1',
      'todo2',
      // oldValue값을 참조하거나 파라미터를 전달하는 경우
      function foo (value, oldValue) {

      },
    ],
  }
}
``` 

3. computed속성으로 해결할 수 있다면 가급적 사용하지 않는다.
