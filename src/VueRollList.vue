<template>
  <div class="roll-list">
    <button class="roll-list-btn" @click="fetchData">
      <i
        :style="{
          transform: `rotate(${counter * 360}deg)`,
          display: 'inline-block',
          transition: '0.5s ease'
        }"
      >
        <slot name="icon">&nbsp;の&nbsp;</slot>
      </i>
      <span>
        <slot name="text">换一换</slot>
      </span>
    </button>
    <slot v-if="setLoading && loading" name="loading" />
    <slot v-else :list="filterList" />
  </div>
</template>

<script>
export default {
  name: 'VueRollList',
  props: {
    list: {
      required: true,
      type: Array
    },
    count: {
      required: true,
      type: Number
    },
    setLoading: {
      type: Boolean,
      default: false
    },
    fetch: {
      type: Function,
      default: null
    }
  },
  data() {
    return {
      loading: false,
      curPage: 1,
      maxPage: 0,
      counter: 0
    }
  },
  computed: {
    filterList() {
      const { list, curPage, count } = this
      const begin = (curPage - 1) * count
      return list.slice(begin, begin + count)
    }
  },
  methods: {
    async fetchData() {
      if (this.loading) {
        return
      }
      if (this.maxPage) {
        this.counter++
        if (this.curPage === this.maxPage) {
          this.curPage = 1
        } else {
          this.curPage++
        }
        return
      }
      if (!this.fetch) {
        return
      }
      const oldLength = this.list.length
      this.loading = true
      this.counter++
      try {
        await this.fetch()
        const newLength = this.list.length - oldLength
        if (newLength) {
          this.curPage++
        }
        if (newLength < this.count) {
          this.maxPage = Math.ceil(this.list.length / this.count)
        }
        if (!newLength) {
          this.curPage = 1
        }
      } finally {
        this.loading = false
      }
    }
  }
}
</script>
