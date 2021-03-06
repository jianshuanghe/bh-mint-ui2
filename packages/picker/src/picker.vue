<template>
  <div class="mint-picker mt-bg-lv3">
    <div class="mint-picker__toolbar mt-bColor-grey-lv6" v-if="showToolbar">
      <slot>
        <div class="mint-picker__cancel mt-color-grey" @click="emit('cancel')"><i v-if="!cancelText" class="iconfont icon-close"></i><span class="mt-color-theme">{{cancelText}}</span></div>
        <div class="mint-picker__confirm  mt-color-theme" @click="emit('confirm')">{{ confirmText }}</div>
        <div class="mint-picker__title  mt-color-grey" v-if="title" v-text="title" />
      </slot>
    </div>
    <div class="mint-picker__columns">
      <mt-picker-column
        v-for="(item, index) in currentColumns"
        :key="index"
        :valueKey="valueKey"
        :options="item.values"
        :className="item.className"
        :defaultIndex="item.defaultIndex"
        :itemHeight="itemHeight"
        :visibileColumnCount="visibileColumnCount"
        :isInterrelated="isInterrelated"
        @change="onChange"
      />
    </div>
  </div>
</template>

<script>
import Column from './PickerColumn';
export default {
  name: 'mt-picker',
  components: {
    [Column.name]: Column
  },
  props: {
    title: String,
    valueKey: {
      type: String,
      default: 'text'
    },
    itemHeight: Number,
    showToolbar: Boolean,
    visibileColumnCount: Number,
    columns: {
      type: Array,
      default: () => []
    },
    cancelText: {
      type: String,
      default: ''
    },
    confirmText: {
      type: String,
      default: '确定'
    },
    isInterrelated: Boolean
  },
  data() {
    return {
      children: [],
      currentColumns: []
    };
  },
  created() {
    this.initColumns();
    this.$nextTick(()=>{
      this.onChange(0);
    });
  },
  watch: {
    columns() {
      this.initColumns();
    },
  },
  methods: {
    initColumns() {
      const columns = JSON.parse(JSON.stringify(this.columns));
      this.isSimpleColumn = columns.length && !columns[0].values;
      this.currentColumns = this.isSimpleColumn ? [{ values: columns }] : columns;
      if(this.isInterrelated){
        let showData = columns;
        let id_1 = "";
        if(showData[0].defaultIndex){
          id_1 = showData[0].values[showData[0].defaultIndex].id;
        }else{
          id_1 = showData[0].values[0].id;
        }
        let childrenColumn=[];
        for(let i=0;i<showData[1].values.length;i++){
          if(showData[1].values[i].pid === id_1) childrenColumn.push(showData[1].values[i]);
        }
        showData[1].values=childrenColumn;
        this.currentColumns = showData;
      }

    },
    emit(event) {
      if (this.isSimpleColumn) {
        this.$emit(event, this.getColumnValue(0), this.getColumnIndex(0), this);
      } else {
        this.$emit(event, this.getValues(), this.getIndexes(), this);
      }
    },
    onChange(columnIndex) {
      if (this.isSimpleColumn) {
        this.$emit('change', this, this.getColumnValue(0), this.getColumnIndex(0));
      } else {
        if(this.isInterrelated){
          const columns = JSON.parse(JSON.stringify(this.columns));
          let id_1 = this.getValues()[0].id;
          let showData = columns;
          let childrenColumn=[];
          for(let i=0;i<showData[1].values.length;i++){
            if(showData[1].values[i].pid === id_1) childrenColumn.push(showData[1].values[i]);
          }
          showData[1].values=childrenColumn;
          this.currentColumns = showData;
        }else{
          this.$emit('change', this, this.getValues(), columnIndex);
        }
      }
    },
    // get column instance by index
    getColumn(index) {
      return this.children[index];
    },
    // get column value by index
    getColumnValue(index) {
      return (this.getColumn(index) || {}).currentValue;
    },
    // set column value by index
    setColumnValue(index, value) {
      const column = this.getColumn(index);
      column && column.setValue(value);
    },
    // get column option index by column index
    getColumnIndex(columnIndex) {
      return (this.getColumn(columnIndex) || {}).currentIndex;
    },
    // set column option index by column index
    setColumnIndex(columnIndex, optionIndex) {
      const column = this.getColumn(columnIndex);
      column && column.setIndex(optionIndex);
    },
    // get options of column by index
    getColumnValues(index) {
      return (this.currentColumns[index] || {}).values;
    },
    // set options of column by index
    setColumnValues(index, options) {
      const column = this.currentColumns[index];
      if (column) {
        column.values = options;
      }
    },
    // get values of all columns
    getValues() {
      return this.children.map(child => child.currentValue);
    },
    // set values of all columns
    setValues(values) {
      values.forEach((value, index) => {
        this.setColumnValue(index, value);
      });
    },
    // get indexes of all columns
    getIndexes() {
      return this.children.map(child => child.currentIndex);
    },
    // set indexes of all columns
    setIndexes(indexes) {
      indexes.forEach((optionIndex, columnIndex) => {
        this.setColumnIndex(columnIndex, optionIndex);
      });
    }
  }
};
</script>
<style lang="css">
  @import "../../../src/style/hairline.css";
  .mint-picker {
    overflow: hidden;
    user-select: none;

    &__toolbar {
      padding: 13px 0;
      font-size: 18px;
      line-height: 1;
      overflow: hidden;
      border-bottom-width: 0.5px;
      border-bottom-style: solid;
    }

    &__cancel,
    &__confirm {
      padding: 0 15px;

      &:active {
        opacity:0.6;
      }
    }

    &__cancel {
      float: left;
      height: 18px;
      .iconfont{
        font-size:30px;
        display:inline-block;
        margin-top:-4px;
      }
    }

    &__confirm {
      float: right;
    }

    &__title {
      text-align: center;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
    }

    &__columns {
      display: flex;
      position: relative;
    }

    &-column {
      flex: 1;
      overflow: hidden;
      font-size: 18px;
      position: relative;
      text-align: center;

      ul {
        box-sizing: border-box;
      }

      li {
        padding: 0 10px;
        overflow: hidden;
        white-space: nowrap;
        text-overflow: ellipsis;
      }

      li&--disabled {
        opacity: .3;
      }

      &__frame {
        top: 50%;
        left: 0;
        width: 100%;
        z-index: 2002;
        position: absolute!important;
        pointer-events: none;
        transform: translate3d(0,-50%,0);
      }
    }
  }
</style>