<template>
  <div class="page-select">

    <h1 class="page-item-title">Select 快捷语法糖</h1>
    <mt-cell-group>
      <mt-select label="单选" :options="singleSelectOptions" v-model="singleSelectValue" select-type="select" @selector-click="singleSelectClick">
      </mt-select>
      <p>{{singleSelectValue}}</p>
    </mt-cell-group>

    <h1 class="page-item-title">Multi Select 快捷语法糖</h1>
    <mt-cell-group>
      <mt-select label="多选" :options="multiSelectOptions" v-model="multiSelectValue" select-type="multi-select" @selector-click="multiSelectClick" iconpattern="hook">
      </mt-select>
      <p>{{multiSelectValue}}</p>
    </mt-cell-group>
<!--     <div class="">
      <mt-select label="单选" :options="yearSlot" :value.sync="value">
        <template scope="scope" slot="display">
          {{scope.value}}
        </template>
        <template scope="scope" slot="selector">
          <mt-cell v-for="item in scope.options" :key="item" :class="{active: item === scope.value}" :title="item" @click.native.stop="selectClick(item)"></mt-cell>
        </template>
      </mt-select>
    </div> -->

<!--     <h1 class="page-title">Multi Select</h1>
    <div class="">
      <mt-select label="多选" :options="yearSlot" :value.sync="multiValue">
        <template scope="scope" slot="display">
          {{scope.value.join(',')}}
        </template>
        <template scope="scope" slot="selector">
          <mt-cell v-for="item in scope.options" :class="{active: scope.value.indexOf(item) > -1 }" :key="item" :title="item" @click.native.stop="multiValue.push(item)"></mt-cell>
        </template>
      </mt-select>
    </div> -->

    <!--<h1 class="page-title">字典 Select</h1>
    <div class="">
      <mt-select label="字典多选" :options="dicSlot" :value.sync="dicValue">
        <template scope="scope" slot="display">
          {{scope.value.map(item => item.name).join(',')}}
        </template>
        <template scope="scope" slot="selector">
          <mt-cell v-for="item in scope.options" :class="{active: scope.value.indexOf(item) > -1 }" :key="item.id" :title="item.name" @click.native.stop="dicValue.push(item)"></mt-cell>
        </template>
      </mt-select>
      <p>取值：{{dicValue.map(item => item.id).join(',')}}</p>
    </div>

    <h1 class="page-title">异步加载数据 Select</h1>
    <div class="">
      <mt-select label="异步字典多选" :options="asyncDicSlot" :value.sync="asynvDicValue" @selector-click="handleDicSelectorClick">
        <template scope="scope" slot="display">
          {{scope.value.map(item => item.name).join(',')}}
        </template>
        <template scope="scope" slot="selector">
          <mt-cell v-for="item in scope.options" :class="{active: scope.value.indexOf(item) > -1 }" :key="item.id" :title="item.name" @click.native.stop="asynvDicValue.push(item)"></mt-cell>
        </template>
      </mt-select>
      <p>取值：{{dicValue.map(item => item.id).join(',')}}</p>
    </div> -->

    <!-- <h1 class="page-title">日期</h1>
    <div class="">
      <mt-select label="异步字典多选" :slots="asyncDicSlot" :value.sync="asynvDicValue" @selector-click="handleDicSelectorClick">
        <template scope="scope" slot="display">
          {{scope.value.map(item => item.name).join(',')}}
        </template>
        <template scope="scope" slot="selector">
          <mt-cell v-for="item in scope.slots[0].values" :class="{active: scope.value.indexOf(item) > -1 }" :key="item.id" :title="item.name" @click.native.stop="asynvDicValue.push(item)"></mt-cell>
        </template>
      </mt-select>
      <p>取值：{{dicValue.map(item => item.id).join(',')}}</p>
    </div> -->


  </div>
</template>

<style lang="postcss">
  @component-namespace page {
    @component select {
      padding-top: 45px;
      @descendent wrapper {
        background-color: #fff;
      }

      @descendent desc {
        margin: 10px 0 50px;
      }

      .mint-button {
        margin-top: 15px;
      }
    }
    @component title {
      font-size: 20px;
      margin: 0 auto;
      padding:20px 0;
      text-align: center;
      display: block;
      line-height: 1; 
    }
    @component item {
      @descendent title {
        text-align: center;
        color: #666;
        margin-bottom: 5px;
      }
    }
  }
</style>

<script type="text/babel">
  /* eslint-disable */
  import axios from 'axios'

  export default {
    methods: {
      selectClick (item) {
        this.value = item
        if (window.location.hash.indexOf('smile-select') > -1) {
          history.back()
        }
      },
      handleDicSelectorClick () {
        axios.get('/mock/userdata.json').then(resp => {
          let respData = resp.data
          if (respData.code == '0') {
            this.$set(this.asyncDicSlot[0], 'values', respData.data)
            this.asyncDicSlot = respData.data
          }
        });
      },
      singleSelectClick () {
        // axios.get('/mock/userdata.json').then(resp => {
        //   let respData = resp.data
        //   if (respData.code == '0') {
        //     // this.$set(this.asyncDicSlot[0], 'values', respData.data)
        //     this.singleSelectOptions = respData.datas.code.rows;
        //     //this.singleSelectOptions = this.singleSelectOptions.slice(10);
        //   }
        // })
        this.singleSelectOptions = [
            {
              "czz": null,
              "name": "安昌珍",
              "sfytb": null,
              "czrq": null,
              "id": "0000001001",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安成守",
              "sfytb": null,
              "czrq": null,
              "id": "0000001002",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安春光",
              "sfytb": null,
              "czrq": null,
              "id": "0000001004",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安冬梅",
              "sfytb": null,
              "czrq": null,
              "id": "0000001005",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安粉玉",
              "sfytb": null,
              "czrq": null,
              "id": "0000001007",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安丰存",
              "sfytb": null,
              "czrq": null,
              "id": "0000001008",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安丰军",
              "sfytb": null,
              "czrq": null,
              "id": "0000001009",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安福林",
              "sfytb": null,
              "czrq": null,
              "id": "0000001011",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安国峰",
              "sfytb": null,
              "czrq": null,
              "id": "0000001013",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安国山",
              "sfytb": null,
              "czrq": null,
              "id": "0000001014",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }
          ]
      },
      multiSelectClick () {
        // axios.get('/mock/userdata.json').then(resp => {
        //   let respData = resp.data
        //   if (respData.code == '0') {
        //     this.multiSelectOptions = respData.datas.code.rows
        //   }
        // })
        this.multiSelectOptions = [
            {
              "czz": null,
              "name": "安昌珍",
              "sfytb": null,
              "czrq": null,
              "id": "0000001001",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安成守",
              "sfytb": null,
              "czrq": null,
              "id": "0000001002",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安春光",
              "sfytb": null,
              "czrq": null,
              "id": "0000001004",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安冬梅",
              "sfytb": null,
              "czrq": null,
              "id": "0000001005",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安粉玉",
              "sfytb": null,
              "czrq": null,
              "id": "0000001007",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安丰存",
              "sfytb": null,
              "czrq": null,
              "id": "0000001008",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安丰军",
              "sfytb": null,
              "czrq": null,
              "id": "0000001009",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安福林",
              "sfytb": null,
              "czrq": null,
              "id": "0000001011",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安国峰",
              "sfytb": null,
              "czrq": null,
              "id": "0000001013",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }, {
              "czz": null,
              "name": "安国山",
              "sfytb": null,
              "czrq": null,
              "id": "0000001014",
              "title": null,
              "sort_num": null,
              "tblx": null,
              "tbrq": null
            }
          ]
      }
    },

    data() {
      return {
        singleSelectValue: '',
        singleSelectOptions: [],

        multiSelectValue: '0000001005,0000001007',
        multiSelectOptions: [],

        value: '1984',
        multiValue: [],
        dicValue: [],
        asynvDicValue: [],
        year: '1984',
        number: 0,
        yearSlot: [ '1984', '1985', '1986', '1987', '1988', '1989', '1990', '1991', '1992', '1993', '1994', '1995' ],
        dicSlot: [
          { id: 1, name: '奔波儿灞' },
          { id: 2, name: '霸波尔奔' },
          { id: 3, name: '金角大王' },
          { id: 4, name: '银角大王' },
          { id: 5, name: '虎力大仙' },
          { id: 6, name: '鹿力大仙' },
          { id: 7, name: '羊力大仙' },
          { id: 8, name: '黄袍怪' },
          { id: 9, name: '白骨精' },
          { id: 10, name: '小钻风' }
        ],
        asyncDicSlot: [],
        numberSlot: [ 0, 1, 2, 3, 4, 5, 6 ],
        addressCity: '北京'
      }
    },

    mounted() {
    }
  };
</script>