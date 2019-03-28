# vue-roll-list

## Project setup
```
yarn install
```

## get start
```shell
yarn add vue-roll-list
```

## usage
```vue
<VueRollList
  v-if="list.length"
  :list="list"
  :fetch="getData"
  :count="count"
  :set-loading="true"
>
  <ul slot-scope="{ list }">
    <li
      v-for="item in list"
      :key="item.id"
      :style="{ width: `${item.width}px`, backgroundColor: item.style.color }"
    >
      {{ item.index + 1 }}
    </li>
  </ul>
  <ul slot="loading">
    <li
      v-for="item in count"
      :key="item"
    >
      loading
    </li>
  </ul>
</VueRollList>
```

| key | value | required | meaning |
| --- | --- | --- | --- |
| list | Array | Y | 数据列表 |
| fetch | Function | Y | 加载下一页数据的函数 |
| count | Number > 0 | Y | 每页展示的个数 |
| set-loading | Boolean | N | 是否开启自定义 loading，默认为 false |

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Run your tests
```
yarn run test
```

### Lints and fixes files
```
yarn run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
