Vue.js
====

## 참조

* 현재 레거시 기준 사용빈도가 많은 스타일을 기준으로 정리 함

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

* Helper가 필요한 경우 아래와 같이 Helper함수를 이용한다.
```javascript
(O)
import { createNamespacedHelpers } from 'vuex';
const { mapState, mapActions } = createNamespacedHelpers('[모듈네임]');

...mapActions([
  '[ActionName]',
]),

(X)
import { mapState, mapActions } from 'vuex'

...mapActions([
  '[모듈네임]/[ActionName]',
]),
```

