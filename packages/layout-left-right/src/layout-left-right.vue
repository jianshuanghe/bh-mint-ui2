<template>
  <div @click="handleClick" class="mint-layout-lr">
    <slot name="left"></slot>
    <slot name="right"></slot>
  </div>
</template>
<script>
    /**
     * @noteType component
     * @name LayoutLeftRight
     * @tagName mt-layout-left-right
     * @desc 左右布局容器
     * @type containerWrap
     * @wrapClassName mint-layout-lr
     * @html
     * <mt-layout-left-right></mt-layout-left-right>
     */
    export default {
        name: 'mt-layout-left-right',
        methods: {
            handleClick(evt) {
                this.$emit('click', evt);
            }
        },
        props: {}
    };

    /**
     * @noteType external
     * @content
     {
       "containerItem": [
          {
            "tagName": "LayoutLeftRightItem",
            "attrs": {
              "slot": "left",
              "position": "left"
            }
          },
          {
            "tagName": "LayoutLeftRightItem",
            "attrs": {
              "slot": "right",
              "position": "right"
            }
          }
        ]
     }
     */
</script>
<style lang="css">
  @component-namespace mint {
    @component layout-lr {
        position: relative;
        display: table;
        width: 100%;
        box-sizing: border-box;
    }
  }

</style>
