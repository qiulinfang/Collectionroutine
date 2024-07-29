<template>
  <view class="table-component">
    <view class="table-header">
      <slot name="header">
        <view v-for="(column, columnIndex) in columns" :key="columnIndex" class="table-column">
          {{ column.label }}
        </view>
      </slot>
    </view>
    <view class="table-body">
      <slot name="body">
        <view v-for="(item, rowIndex) in displayedData" :key="rowIndex" class="table-row">
          <slot name="cell" v-for="(column, columnIndex) in columns" :key="columnIndex" :item="item" :column="column">
            {{ item[column.field] }}
          </slot>
          <slot name="expand" v-if="expandable && expandedRowKeys.includes(item.id)">
            <view class="expanded-row">
              <slot name="expanded-content" :item="item">
                扩展内容
              </slot>
            </view>
          </slot>
        </view>
      </slot>
    </view>
    <view class="table-footer">
      <slot name="footer">
        <view class="pagination">
          <slot name="pagination">
            分页控件
          </slot>
        </view>
      </slot>
    </view>
  </view>
</template>

<script>
export default {
  name: 'TableComponent',
  props: {
    data: {
      type: Array,
      required: true
    },
    columns: {
      type: Array,
      required: true
    },
    sort: {
      type: Object,
      default: () => ({})
    },
    filter: {
      type: Object,
      default: () => ({})
    },
    pagination: {
      type: Object,
      default: () => ({})
    },
    expandable: {
      type: Boolean,
      default: false
    },
    expandedRowKeys: {
      type: Array,
      default: () => []
    }
  },
  computed: {
    displayedData() {
      let data = [...this.data];
      // 这里可以添加排序和筛选逻辑
      return data;
    }
  },
  methods: {
    sort(column, direction) {
      // 排序逻辑
    },
    filter(column, value) {
      // 筛选逻辑
    },
    paginate(page, pageSize) {
      // 分页逻辑
    },
    expand(row) {
      // 展开行逻辑
    },
    collapse(row) {
      // 折叠行逻辑
    },
    editCell(row, field) {
      // 开始编辑单元格逻辑
    },
    stopEditCell(row, field) {
      // 结束编辑单元格逻辑
    },
    selectAllRows() {
      // 选择所有行逻辑
    },
    deselectAllRows() {
      // 取消选择所有行逻辑
    }
  }
};
</script>

<style scoped>
.table-component {
  display: flex;
  flex-direction: column;
}

.table-header {
  display: flex;
  border-bottom: 1px solid #ddd;
}

.table-column {
  flex: 1;
  padding: 8px;
  text-align: center;
}

.table-body {
  display: flex;
  flex-direction: column;
}

.table-row {
  display: flex;
  border-bottom: 1px solid #ddd;
}

.table-cell {
  flex: 1;
  padding: 8px;
  text-align: center;
}

.expanded-row {
  padding: 8px;
  background-color: #f9f9f9;
}

.pagination {
  padding: 8px;
  text-align: center;
}
</style>