# password
A powerful password validator that supports bootstrap4 by default.


## Installation
```bash
npm install ofcold-password --save
```

## Usage

```vue
	<OfcoldPassword v-model="password"></OfcoldPassword>
```

```js
import Vue from 'vue';
import BootstrapVue from 'bootstrap-vue'
import OfcoldPassword from 'ofcold-password';
Vue.use(BootstrapVue);
Vue.use(OfcoldPassword);

```

OR
```js
import Vue from 'vue'
import BootstrapVue from 'bootstrap-vue'
import OfcoldPassword from 'ofcold-password/src/App';
Vue.use(BootstrapVue);

export default {
	components: {
		OfcoldPassword
	}
}

```

![Preview]('./pass.gif')