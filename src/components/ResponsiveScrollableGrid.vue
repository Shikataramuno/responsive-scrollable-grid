<template>
  <div class="container-fluid">
    <!-- #### PC用 #### -->
    <div class="pc table-row header">
      <b-row>
        <label class="title" >メンバ一覧 </label>
      </b-row>
      <b-row class='query-box'>
        <b-col cols="2">
          <form id="search">
            <input name="query" class="filter" v-model="searchQuery" placeholder="フィルタ文字列">
          </form>
        </b-col>
        <b-col cols="10">
        </b-col>
      </b-row>
      <div class="wrapper attributes header">
        <div v-for="(val, idx) in columns" v-bind:key=idx @click="sortBy(val)" :class="[{ active: sortKey === val }, val]">
          {{ val }}
          <span class="arrow" :class="sortOrders[val] > 0 ? 'asc' : 'dsc'"></span>
        </div>
      </div>
    </div>
    <!-- #### スマホ用 #### -->
    <div class="mobile">
      <b-container class="table-row header">
        <b-row>
          <label class="title" >メンバ一覧 </label>
        </b-row>
        <b-row>
          <b-col cols="4">
            <input name="query" class="filter" v-model="searchQuery" placeholder="フィルタ文字列">
          </b-col>
          <b-col cols="4">
            <b-dropdown id="ddown-buttons" split right variant="success" size="sm" class="sorter">
              <template slot="button-content">
                {{sortKey}}
                <span class="arrow" :class="sortOrders[sortKey] > 0 ? 'asc' : 'dsc'"></span>
              </template>
              <b-dropdown-item v-for="(val, idx) in columns" v-bind:key=idx @click="sortBy(val)" :class="[{ active: sortKey == val }, { focus: sortKey == val }]">
                {{ val }}
              </b-dropdown-item>
            </b-dropdown>
          </b-col>
        </b-row>
      </b-container>
    </div>

    <div class="data-field">
      <div v-for="(entry,idx) in members" v-bind:key=idx>
        <div class="table-row data" v-bind:style="[selectedId===entry.id ? styleForSelectedRow : idx%2 === 0 ? styleForNonSelectedEvenRow : styleForNonSelectedOddRow]" @click="edit(entry.id)">
          <div class="wrapper attributes data">
            <div v-for="(val, idx) in columns" v-bind:key=idx :class="[val]">
              <span class='mobile-title'>{{val}}:</span>
              <span>
                {{entry[val]}}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script lang='ts'>
import { Component, Prop, Vue } from 'vue-property-decorator';
import Member from '../models/Member';
import SortOrders from '../models/SortOrders';

@Component
export default class ResponsiveScrollableGrid extends Vue {
  name: string = 'ResponsiveScrollableGrid';
  userName: string = 'admin';
  searchQuery: string = '';
  sortKey: string = 'キー';
  memberList: Member[] = [];
  columns: string[] = ['id', 'name', 'address'];
  sortOrders: SortOrders = new SortOrders();
  selectedId: number = -1;
  styleForSelectedRow: object = {'background-color': '#C0C0C0'};
  styleForNonSelectedEvenRow: object = {'background-color': '#FFFFFF'};
  styleForNonSelectedOddRow: object = {'background-color': '#F5F5F5'};

  // computed
  get members(): Member[] {
    let ret = this.memberList;
    const filterKey = this.searchQuery && this.searchQuery.toLowerCase();
    if (filterKey) {
      ret = ret.filter((row) => {
        return row.isIncluded(filterKey);
      });
    }
    const order = this.sortOrders.getOrder(this.sortKey) || -1;
    ret = ret.slice().sort((a: Member, b: Member) => {
      const aVal: string = a.getValue(this.sortKey);
      const bVal: string = b.getValue(this.sortKey);
      return (aVal === bVal ? 0 : aVal > bVal ? 1 : -1) * order;
    });
    return ret;
  }

  created(): void {
    console.log('created');
    this.memberList = [
     new Member(1, 'aaaa', 'aaaa@shikataramuno.com'),
     new Member(2, 'bbbb', 'bbbb@shikataramuno.com'),
     new Member(3, 'cccc', 'cccc@shikataramuno.com'),
     new Member(4, 'dddd', 'dddd@shikataramuno.com'),
     new Member(5, 'eeee', 'eeee@shikataramuno.com'),
     new Member(6, 'ffff', 'ffff@shikataramuno.com'),
     new Member(7, 'gggg', 'gggg@shikataramuno.com'),
     new Member(8, 'hhhh', 'hhhh@shikataramuno.com'),
     new Member(9, 'iiii', 'iiii@shikataramuno.com'),
     new Member(10, 'jjjj', 'jjjj@shikataramuno.com'),
     new Member(11, 'kkkk', 'kkkk@shikataramuno.com'),
     new Member(12, 'llll', 'llll@shikataramuno.com'),
     new Member(13, 'mmmm', 'mmmm@shikataramuno.com'),
     new Member(14, 'nnnn', 'nnnn@shikataramuno.com'),
     new Member(15, 'oooo', 'oooo@shikataramuno.com'),
     new Member(16, 'pppp', 'pppp@shikataramuno.com'),
     new Member(17, 'qqqq', 'qqqq@shikataramuno.com'),
     new Member(18, 'rrrr', 'rrrr@shikataramuno.com'),
     new Member(19, 'ssss', 'ssss@shikataramuno.com'),
     new Member(20, 'tttt', 'tttt@shikataramuno.com'),
     new Member(21, 'uuuu', 'uuuu@shikataramuno.com'),
     new Member(22, 'vvvv', 'vvvv@shikataramuno.com'),
     new Member(23, 'wwww', 'wwww@shikataramuno.com'),
     new Member(24, 'xxxx', 'xxxx@shikataramuno.com'),
     new Member(25, 'yyyy', 'yyyy@shikataramuno.com'),
     new Member(26, 'zzzz', 'zzzz@shikataramuno.com'),
    ];
  }
  mounted(): void {
    console.log('mounted');
  }

  sortBy(key: string): void {
    this.sortKey = key;
    this.sortOrders.selectKey(this.sortKey);
  }
  edit(id: number): void {
    this.selectedId = id;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container-fluid{
  padding: 5px;
  width: 100%;
}
.pc {
  font-size: 80%;
  font-weight: bold;
  display: block;
}
.mobile {
  font-size: 80%;
  font-weight: bold;
  display: none;
}
.table-row {
  border-bottom: 1px solid #e0e0e0;
  border-collapse: collapse;
  margin-left: 0px;
  margin-right: auto;
}
.table-row.header {
  background-color: rgb(229, 255, 219);
  font-weight: bold;
  padding-top: 6px;
  padding-bottom: 6px;
  padding-left: 4px;
}
.row {
  margin-left: 0px;
}
.title {
  font-size: small;
  color:  rgb(26, 92, 0);
  margin-left: 1em;
}
.filter {
  width: 100%;
}
.query-box {
  margin-bottom: 1em
}
.wrapper {
  display: flex;
  display: -webkit-flex;
  flex-direction: row;
  -webkit-flex-direction: row;
}
.attributes {
  flex-grow: 0;
  -webkit-flex-grow: 0;
}
.wrapper.attributes.header {
  height: 20px;
}

.data-field {
  height: 600px;
  overflow-y: auto;
}
.table-row.data {
  height: auto;
  width: 100%;
  padding-left: 6px;
  padding-right: 6px;
}
.wrapper.attributes.data {
  height: auto;
  margin-left: 0px;
  margin-right: 0px;
  padding-top: 6px;
}
.mobile-title {
  display: none;
}
.id {
  width: 100px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  text-align: left;
}
.name {
  width: 200px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  text-align: left;
}
.address {
  width: 300px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  text-align: left;
}

/*
 * Media queries: optimize for different screen widths.
 */
@media screen and (max-device-width: 768px),screen and (max-width: 768px)
{
  .container {
    padding-left: 4px;
  }
  .pc {
    font-size: 80%;
    font-weight: bold;
    display: none;
  }
  .mobile {
    font-size: 80%;
    font-weight: bold;
    display: inline;
  }
  .wrapper.attributes.header {
    height: auto;
  }
  .filter {
    width: 100%;
    font-size: 16px;
    /* transform: scale(0.8); */
  }
  .sorter {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translateY(-50%) translateX(-50%);
    -webkit-transform: translateY(-50%) translateX(-50%);
    /* float: right; */
  }
  .dropdown .dropdown-menu .dropdown-item:focus {
    outline: none;
    /*
    background-color: #eaeaea;
    color: #1d1e1f;
    */
  }
  .data-field {
    height: 600px;
    overflow-y: auto;
  }
  .wrapper.attributes.data {
    height: auto;
  }
  .mobile-title {
    display: inline;
  }
  .attributes {
    flex-direction: column;
    -webkit-flex-direction: column;
  }
  .attributes div {
    flex-grow: 0;
    -webkit-flex-grow: 0;
  }
  .attributes > div {
    width: 100%;
  }
}
/*
 * General good-look styles
 */
div.active {
  color: rgb(55, 11, 177);
}
.arrow {
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.0;
}
.arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid #000;
}
.arrow.dsc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid #000;
}
.sorter .arrow {
  opacity: 1;
}
div.active .arrow {
  opacity: 1;
}
div.active .arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid rgb(55, 11, 177);
}
div.active .arrow.dsc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid rgb(55, 11, 177);
}
</style>
