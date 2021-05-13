Vue.js
====

## 참조

* 현재 기준 사용빈도가 많은 스타일을 기준으로 정리 함

## template

* 태그의 명명과 프로퍼티바인딩은 스네이크케이스로 표기한다
```html
(O)  <eight-percent />
(X)  <EightPercent />
``` 

* Slot영역이 필요없는 경우 축양형으로 표기한다.
```html
(O)  <eight-percent />
(X)  <EightPercent></EightPercent>
```

* 프로퍼티와 어트리뷰트 나열은 줄바꿈 이후 선언한다.
```html
(O)
<eight-percent
  v-if="loaded"
  :data-index="dealIndex"
/>

(X)
<eight-percent v-if="loaded" :data-index="dealIndex"/>
```

* 단 순수한 HTML태그에는 많은 요소가 바인딩되지 않는다면 한줄선언을 허용한다
```html
(O)
<div id="foo" class="bar"></div>
(-)
<div
  class="x"
></div>
```

* 프로퍼티와 어트리뷰트 나열은 다음의 순서로 진행한다.
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
* 이벤트바인딩은 반드시 카멜케이스로 작성한다. (케밥케이스 작성 시 미작동)

## Style

* 레이아웃, 믹스인, 변수 등 전역 스타일을 제외한 영역에서 scoped scss를 사용한다 `<style lang="scss" scoped>`
* 클래스의 명명은 BEM을 따른다.
* ID선택자는 사용하지 않는다.

## Component

* 컴퍼넌트의 구성은 아래와 같은 항묵으로 구분된다.
  * layouts  : 전체적인 구조 구성을 위한 컴퍼넌트
  * pages    : 하나의 라우터를 가지는 페이지 단위 컴퍼넌트
  * modules  : page 단위에서 공통으로 사용되는 모듈화된 컴퍼넌트
  * components : 어플리케이션 전체에서 사용되는 모듈화된 컴퍼넌트

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
* Store의 사용은 Helper함수를 사용하지 않고 기본적인 기능을 사용한다.
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

  mutations: {
    setState(state, value) {
      state.dealNotice = value;
    },
  },

  actions: {
    fetchData(context, id) {
      return request
        .get(`/api/boards/deal-notices/`)
        .type('json')
        .accept('json')
        .then((res) => {
          context.commit('setState', res.body);
        });
    },
  },
};

```

* Store
  * Action은 비동기과정이나 유저의 행위를 정의하여 사용한다.
  * 컴퍼넌트에서 상태변화를 일으킬때는 mutation보다는 action으로 접근하도록 한다.

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

## 개선할점
1. Watch의 경우 행위를 Methods로 분리하면 좋을 것 같다.
```javascript
// as-is

watch: {
  watchDataName() {
    // ...긴 로직
  },
},

// to-be
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

2. 태그명명과 props바인딩은 케밥케이스보다는 카멜케이스가 혼용되어있고 케밥케이스를 선호하는데 카멜케이스가 더 적합한 것 같다. 
   * 이유는 HTML테그와 vue component를 분리하여 볼 수 있다.
   * 이벤트바인딩의 경우 카멜케이스가 필수값이라 하나로 맞추는게 보기에 좋다.
```javascript
<datetime-select
   :startDate="foo"
   :end-date="bar"
/>

<DatetimeSelect
  :startDate="foo"
/>
```

1. v-model보다는 value를 직접 관리하는 것이 좋을 것 같다. 

2. /deep/ selector will be deprecated soon 
   * scss컴파일 과정에서 최종 css에서는 실제 사용되지는 않으나 일반적으로 사용하지 않는 속성으로 대체할 필요성이 있음 (대안: 스타일바인딩 또는 `::v-deep`, `>>>`)

