<template>
  <div :class="{'hidden':total<=limit}" class="pagination-container">
    <el-pagination :small="isSmall" :background="background" :current-page.sync="currentPage" :page-size.sync="pageSize" :layout="smallLayout" :total="total" v-bind="$attrs" @size-change="handleSizeChange" @current-change="handleCurrentChange" />
  </div>
</template>

<script>
import { scrollTo } from '@/utils/scroll-to'

export default {
  name: 'XnPage',
  props: {
    isSmall: {
      type: Boolean,
      default: false
    },
    total: {
      required: true,
      type: Number
    },
    page: {
      type: Number,
      default: 1
    },
    limit: {
      type: Number,
      default: 1
    },
    pageSizes: {
      type: Array,
      default() {
        return [10, 15, 20, 30, 50]
      }
    },
    layout: {
      type: String,
      default: 'total, prev, pager, next, jumper'
    },
    background: {
      type: Boolean,
      default: true
    },
    autoScroll: {
      type: Boolean,
      default: true
    },
    hidden: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    smallLayout() {
      return this.isSmall ? 'total, prev, pager, next' : this.layout
    },
    currentPage: {
      get() {
        return this.page
      },
      set(val) {
        this.$emit('update:page', val)
      }
    },
    pageSize: {
      get() {
        return this.limit
      },
      set(val) {
        this.$emit('update:limit', val)
      }
    }
  },
  methods: {
    handleSizeChange(val) {
      this.$emit('pagination', { page: this.currentPage, limit: val })
      if (this.autoScroll && !this.isSmall) {
        scrollTo(0, 800)
      }
    },
    handleCurrentChange(val) {
      this.$emit('pagination', { page: val, limit: this.pageSize })
      if (this.autoScroll && !this.isSmall) {
        scrollTo(0, 800)
      }
    }
  }
}
</script>

<style scoped>
.pagination-container {
  /* background: #fff; */
  margin-top: 20px;
  text-align: right;
}
.pagination-container.hidden {
  display: none;
}
</style>
