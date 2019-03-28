<style lang="scss">
#app {
  position: relative;

  .roll-list {
    padding-top: 40px;
    margin-top: -40px;

    &-btn {
      position: absolute;
      right: 0;
      top: 5px;
    }
  }

  ul {
    list-style-type: none;
    margin: 0;
    padding: 0;

    li {
      display: block;
      height: 100px;
      line-height: 100px;
      margin: 10px 0;
      padding-left: 50px;
      background-color: #d3d3d3;
    }
  }
}
</style>

<template>
  <div id="app">
    <h3>这是标题</h3>
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
  </div>
</template>

<script>
import VueRollList from '../src/VueRollList'
import ItemFactory from './item-factory'

export default {
  name: 'app',
  components: {
    VueRollList
  },
  data() {
    const count = 5
    return {
      count,
      list: ItemFactory.get(count)
    }
  },
  methods: {
    getData() {
      return new Promise(resolve => {
        if (this.list.length >= this.count * 10) {
          return resolve()
        }
        setTimeout(() => {
          this.list = this.list.concat(ItemFactory.get(this.count))
          resolve()
        }, 500)
      })
    }
  }
}
</script>
