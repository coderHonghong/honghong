<!--将data中的cols的prop修改为后端返回的对应字段名称-->
<template>
    <div class="app-container">
        <div class="head-btn-box">
            <div>
                <el-button type="primary" :size="common.btnSize"><i class="el-icon-upload el-icon--left"></i>云入库</el-button>
                <el-button type="primary" :size="common.btnSize" @click="add()"><i class="el-icon-plus el-icon--left"></i>新增</el-button>
                <el-dropdown>
                    <el-button type="primary" :size="common.btnSize">操作<i class="el-icon-caret-bottom el-icon--right"></i></el-button>
                    <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item>资产验收</el-dropdown-item>
                        <el-dropdown-item>领用</el-dropdown-item>
                        <el-dropdown-item>借用</el-dropdown-item>
                        <el-dropdown-item>领用退库</el-dropdown-item>
                        <el-dropdown-item>借用归还</el-dropdown-item>
                        <el-dropdown-item>变更领用人</el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
                <el-dropdown>
                    <el-button :size="common.btnSize">编辑<i class="el-icon-caret-bottom el-icon--right"></i></el-button>
                    <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item>修改</el-dropdown-item>
                        <el-dropdown-item>删除</el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
                <el-dropdown>
                    <el-button :size="common.btnSize"><i class="el-icon-printer el-icon--left"></i>打印<i class="el-icon-caret-bottom el-icon--right"></i></el-button>
                    <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item>打印资产标签</el-dropdown-item>
                        <el-dropdown-item>打印入库单</el-dropdown-item>
                        <el-dropdown-item>打印领用单</el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
                <el-dropdown>
                    <el-button :size="common.btnSize">导入/导出<i class="el-icon-caret-bottom el-icon--right"></i></el-button>
                    <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item>下载导入模板</el-dropdown-item>
                        <el-dropdown-item>批量导入资产</el-dropdown-item>
                        <el-dropdown-item divided>导出查询结果</el-dropdown-item>
                        <el-dropdown-item >导入全部资产</el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
            </div>
           <div>
               <el-button class="margin-right" type="text" @click="showData()"><i class="el-icon-search el-icon-left"></i>高级搜索</el-button>
               <el-dropdown>
                   <el-button type="text">列<i class="el-icon-caret-bottom el-icon--right"></i></el-button>
                   <el-dropdown-menu slot="dropdown">
                       <el-checkbox-group v-model="checkedCities">
                           <el-dropdown-item v-for="item in cities">
                               <el-checkbox  :label="item.prop" :key="item.prop"> {{item.label}}</el-checkbox>
                           </el-dropdown-item>
                       </el-checkbox-group>
                   </el-dropdown-menu>
               </el-dropdown>
           </div>
        </div>

        <el-table :data="tableData" style="width: 100%">
            <el-table-column
                    type="selection"
                    width="35">
            </el-table-column>
            <el-table-column v-for="col in cols" :prop="col.prop" :label="col.label" :width="col.width">
            </el-table-column>
        </el-table>

        <!--新增资产对话框-->
        <addForm  ref="form" :is-add="isAdd"></addForm>

        <!--高级搜索-->
        <searchForm></searchForm>

    </div>
</template>

<script>
  import addForm from './addForm'
  import searchForm from './searchForm'

  const cityOptions = ['date', 'name', 'address']; //所有的数据
  export default {
    components:{
      addForm,
      searchForm
    },
    data() {
      return {
        isAdd: false,
        centerDialogVisible: false,//新增资产对话框，true显示，false隐藏
        checkAll: true,  //默认全选
        checkedCities: ['date', 'name', 'address'], //默认全选所有的数据
        cities: [{
          prop: 'date',
          label: '资产状态'
        },
          {
            prop: 'name',
            label: '资产编码'
          },
          {
            prop: 'address',
            label: '地址'
          },
        ],   //对象数组全部的数据
        isIndeterminate: false,   //默认全选
        dialogFormVisible: false,
        tableData: [{
          status: '领用',
          code: '010102222',
          type: '显示器'
        },
          {
            status: '激活',
            code: '010111002',
            type: '键盘'
          }
        ],
        cols: [{
          prop: 'status',
          label: '资产状态',
          width:100
        },
          {
            prop: 'code',
            label: '资产编码',
            width:100
          },
          {
            prop: 'name',
            label: '资产名称',
            width:100
          },
          {
            prop:'type',
            label:'资产分类',
            width:100
          },
          {
            prop:'',
            label:'品牌',
            width:100
          },
          {
            prop:'',
            label:'型号',
            width:100
          },
          {
            prop:'',
            label:'设备序列号',
            width:100
          },
          {
            prop:'',
            label:'所属/承租公司',
            width:130
          },
          {
            prop:'',
            label:'使用人',
            width:80
          },
          {
            prop:'',
            label:'人员编号',
            width:100
          },
          {
            prop:'',
            label:'手机',
            width:100
          },
          {
            prop:'',
            label:'邮箱',
            width:100
          },
          {
            prop:'',
            label:'使用公司',
            width:100
          },
          {
            prop:'',
            label:'使用部门',
            width:100
          },
          {
            prop:'',
            label:'领用借用日期',
            width:130
          }
        ],
        tempCols: [],  //临时的数据容器
      };
    },
    methods: {
      showData(row) {
        this.dialogFormVisible = true;
      },
      add(){
        this.isAdd = true
        this.$refs.form.dialog = true
      }
    }
  };
</script>


<style scoped>

</style>
