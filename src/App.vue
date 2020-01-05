<template>
  <div id="app">
    <el-container>
      <el-aside width="200px">
        <draggable class="list1-group"
                   :list="list1"
                   :group="group1"
                   ghost-class="ghost"
                   tag="ul">
          <li v-for="(list1Item,list1Idx) in list1" :key="list1Idx" class="list1-group-item">
            {{list1Item.value}}
          </li>
        </draggable>
      </el-aside>
      <el-main>
        <draggable class="list2-group" :list="list2"
                   ghost-class="ghost"
                   chosen-class="chosen"
                   ref="list2"
                   :group="group2"
                   :move="onMove"
                   @onChoose="onChoose"
                   @start="onStart"
                   @change="onChange"
                   @remove="onRemove"
                   @sort="onSort"
                   @add="onAdd"
                   @end="onEnd"
                   tag="ul">
          <li v-for="(list2Item,list2Idx) in list2" :key="list2Idx" class="list2-group-item">
            {{list2Item.value}}
          </li>
        </draggable>
        <draggable class="list3-group" :list="list3"
                   ghost-class="ghost"
                   chosen-class="chosen"
                   :group="group3"
                   :move="onMove"
                   @onChoose="onChoose"
                   @start="onStart"
                   @change="onChange"
                   @remove="onRemove"
                   @sort="onSort"
                   @end="onEnd"
                   tag="ul">
          <li v-for="(list3Item,list3Idx) in list3" :key="list3Idx" class="list3-group-item">
            {{list3Item.value}}
          </li>
        </draggable>
        <el-table
          v-loading="tableLoading"
          :data="tableData"
          style="width: 100%">
          <el-table-column
            v-for="(item,idx) in list2"
            :key="idx"
            :prop="item.key"
            :label="item.value"
            width="180">
          </el-table-column>
        </el-table>
      </el-main>
    </el-container>
  </div>
</template>

<script>
  import draggable from 'vuedraggable';

  export default {
    name: 'app',
    data() {
      return {
        list1: [
          {
            key: 'name',
            value: '姓名A'
          },
          {
            key: 'sex',
            value: '性别B'
          },
          {
            key: 'age',
            value: '年龄C'
          },
          {
            key: 'height',
            value: '身高D'
          },
          {
            key: 'weight',
            value: '体重E'
          },
        ],
        list2: [
          {
            key: 'name',
            value: '姓名A'
          },
          {
            key: 'sex',
            value: '性别B'
          }
        ],
        list3: [
          {
            key: 'name',
            value: '姓名A'
          },
          {
            key: 'sex',
            value: '性别B'
          }
        ],
        group1: {
          name: 'people',
          pull: 'clone',
          put: false
        },
        group2: {
          name: 'people'
        },
        group3: {
          name: 'people'
        },
        tableData: [
          {
            name: 'Jack',
            sex: '男',
            age: '18',
            height: '180',
            weight: '70kg'
          },
          {
            name: 'lucy',
            sex: '女',
            age: '19',
            height: '178',
            weight: '59kg'
          },
          {
            name: 'adam',
            sex: '男',
            age: '21',
            height: '170',
            weight: '66kg'
          },
          {
            name: 'John',
            sex: '男',
            age: '22',
            height: '165',
            weight: '60kg'
          }
        ],
        tableLoading: false,
        isOutsideDrop: false
      };
    },
    components: {
      draggable
    },
    methods: {
      onMove(evt) {
        console.log('move', evt);
      },
      onRemove(e) {
        console.log('remove', e);
        this.isOutsideDrop = false;
      },
      onStart(e) {
        console.log('start', e);
        this.isOutsideDrop = true;
      },
      onChange(e) {
        console.log('change', e);
        this.tableLoading = true;
        setTimeout(() => {
          this.tableLoading = false;
        }, 1000);
      },
      onChoose(e) {
        console.log('choose', e);
      },
      onEnd(e) {
        console.log('end', e);
        console.log(this.isOutsideDrop);
        if (this.isOutsideDrop) {
          let arrName = e.item.className.split('-')[0];
          this[arrName].splice(e.oldIndex, 1);
        }
      },
      onSort(e) {
        console.log('sort', e);
      },
      onAdd(e) {
        console.log(e);
        this.isOutsideDrop = false;
      }
    },
    mounted() {
      document.documentElement.addEventListener('dragover', function (evt) {
        evt.preventDefault();
      });
      this.$refs.list2.$el.addEventListener('drop', function (evt) {
        this.isOutsideDrop = false;
      });
    }
  };
</script>

<style lang="scss">
  ul, ol, sup, h3, p {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
  }

  ol, ul {
    list-style: none;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;

    .list1-group {
      border: 2px solid #e4393c;

      &-item {
        background-color: #ffffff;
        margin-bottom: 10px;

        &.ghost {
          opacity: 0.5;
          background-color: red;
        }
      }
    }

    .list2-group {
      display: flex;
      margin-bottom: 130px;
      border: 2px solid #e4393c;

      &-item {
        background-color: #ffffff;
        margin-right: 30px;
        padding: 5px 20px;

        &.ghost {
          opacity: 0.5;
          background-color: green;
        }

        &.chosen {
          color: green;
        }
      }
    }

    .list3-group {
      display: flex;
      border: 2px solid #e4393c;
      margin-bottom: 100px;

      &-item {
        background-color: #ffffff;
        margin-right: 30px;
        padding: 5px 20px;

        &.ghost {
          opacity: 0.5;
          background-color: blue;
        }

        &.chosen {
          color: blue;
        }
      }
    }
  }
</style>
