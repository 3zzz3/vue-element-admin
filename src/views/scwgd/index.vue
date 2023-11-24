<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input v-model="listQuery.shengChanJiHua" placeholder="生产计划" style="width: 210px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.shengChanZuoYe" placeholder="生产作业" style="width: 210px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.chanPing" placeholder="产品" style="width: 210px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-date-picker
        v-model="listQuery.wanGongRiQi"
        style="width: 400px;"
        class="filter-item"
        type="daterange"
        align="right"
        unlink-panels
        range-separator="至"
        start-placeholder="完工开始日期"
        end-placeholder="完工结束日期"
        :picker-options="wanGongRiQi"
        @keyup.enter.native="handleFilter"
      />
      <el-input v-model="listQuery.shengChanPiHao" placeholder="生产批号" style="width: 210px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-select v-model="listQuery.zhuangTai" style="width: 200px;" class="filter-item" placeholder="状态" @keyup.enter.native="handleFilter">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
      <el-button v-waves class="filter-item" type="primary" icon="el-icon-search" @click="handleFilter">
        搜索
      </el-button>
      <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="handleCreate">
        新增
      </el-button>
      <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="el-icon-refresh" @click="handleQuery">
        重置
      </el-button>
    </div>

    <el-table
      :key="tableKey"
      v-loading="listLoading"
      :data="list"
      border
      fit
      highlight-current-row
      style="width: 100%;"
      @sort-change="sortChange"
    >
      <el-table-column label="编号" prop="id" align="center" width="70">
        <template slot-scope="{row}">
          <span>{{ row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column label="单号" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.danHao }}</span>
        </template>
      </el-table-column>
      <el-table-column label="生产计划" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.shengChanJiHua }}</span>
        </template>
      </el-table-column>
      <el-table-column label="生产作业" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.shengChanZuoYe }}</span>
        </template>
      </el-table-column>
      <el-table-column label="产品" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.chanPing }}</span>
        </template>
      </el-table-column>
      <el-table-column label="型号" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.xingHao }}</span>
        </template>
      </el-table-column>
      <el-table-column label="规格" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.guiGe }}</span>
        </template>
      </el-table-column>
      <el-table-column label="单位" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.danWei }}</span>
        </template>
      </el-table-column>
      <el-table-column label="完工日期" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.wanGongRiQi }}</span>
        </template>
      </el-table-column>
      <el-table-column label="生产数量" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.shengCHAnSHuLiang }}</span>
        </template>
      </el-table-column>
      <el-table-column label="生产批号" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.shengChanPiHao }}</span>
        </template>
      </el-table-column>
      <el-table-column label="状态" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.zhuangTai }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="175" class-name="small-padding fixed-width">
        <template slot-scope="{row,$index}">
          <el-button type="primary" size="mini" @click="handleUpdate(row)">
            详情
          </el-button>
          <el-button v-if="row.status!='deleted'" size="mini" type="danger" @click="handleDelete(row,$index)">
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />

    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
      <el-form ref="dataForm" :rules="rules" :model="temp" label-position="left" label-width="90px" style="width: 400px; margin-left:50px;">
        <el-form-item label="生产批号" prop="shengChanPiHao">
          <el-input v-model="temp.shengChanPiHao" />
        </el-form-item>
        <el-form-item label="备注">
          <el-input v-model="temp.beiZhu" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">
          取消
        </el-button>
        <el-button type="primary" @click="dialogStatus==='create'?createData():updateData()">
          确定
        </el-button>
      </div>
    </el-dialog>

    <el-dialog :visible.sync="dialogPvVisible" title="Reading statistics">
      <el-table :data="pvData" border fit highlight-current-row style="width: 100%">
        <el-table-column prop="key" label="Channel" />
        <el-table-column prop="pv" label="Pv" />
      </el-table>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogPvVisible = false">Confirm</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { fetchList, fetchPv, createArticle, updateArticle } from '@/api/article'
import waves from '@/directive/waves' // waves directive
import { parseTime } from '@/utils'
import Pagination from '@/components/Pagination' // secondary package based on el-pagination

const calendarTypeOptions = [
  { key: 'CN', display_name: 'China' },
  { key: 'US', display_name: 'USA' },
  { key: 'JP', display_name: 'Japan' },
  { key: 'EU', display_name: 'Eurozone' }
]

// arr to obj, such as { CN : "China", US : "USA" }
const calendarTypeKeyValue = calendarTypeOptions.reduce((acc, cur) => {
  acc[cur.key] = cur.display_name
  return acc
}, {})

export default {
  name: 'ComplexTable',
  components: { Pagination },
  directives: { waves },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    },
    typeFilter(type) {
      return calendarTypeKeyValue[type]
    }
  },
  data() {
    return {
      tableKey: 0,
      list: null,
      total: 0,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 20,
        importance: undefined,
        title: undefined,
        type: undefined,
        sort: '+id'
      },
      importanceOptions: [1, 2, 3],
      calendarTypeOptions,
      sortOptions: [{ label: 'ID Ascending', key: '+id' }, { label: 'ID Descending', key: '-id' }],
      statusOptions: ['published', 'draft', 'deleted'],
      showReviewer: false,
      temp: {
        id: undefined,
        importance: 1,
        remark: '',
        timestamp: new Date(),
        title: '',
        type: '',
        status: 'published'
      },
      dialogFormVisible: false,
      dialogStatus: '',
      textMap: {
        update: '修改产品检验单',
        create: '添加生产采样单'
      },
      dialogPvVisible: false,
      pvData: [],
      rules: {
        shengChanPiHao: [{ required: true, message: '必填', trigger: 'blur' }]
      },
      downloadLoading: false,
      ShengChanDate: {
        shortcuts: [{
          text: '最近一周',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近一个月',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
            picker.$emit('pick', [start, end])
          }
        }, {
          text: '最近三个月',
          onClick(picker) {
            const end = new Date()
            const start = new Date()
            start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
            picker.$emit('pick', [start, end])
          }
        }]
      },
      value1: '',
      options: [{
        value: '选项1',
        label: '未入库'
      }, {
        value: '选项2',
        label: '入库中'
      }, {
        value: '选项3',
        label: '已入库'
      }, {
        value: '选项4',
        label: '检验合格'
      }, {
        value: '选项5',
        label: '检验未合格'
      }],
      value: ''
    }
  },
  created() {
    this.getList()
  },
  methods: {
    handleQuery() {
      this.listQuery = {
        liuShui: '',
        wanGongDan: '',
        jiHua: '',
        zuoYe: '',
        ChanPing: '',
        jianYan: '',
        ShengChanDate: '',
        jianYanXiang: '',
        jianYanJieGuo: '',
        jianYanRen: '',
        jianYantime: ''
      }
    },
    getList() {
      this.listLoading = true
      fetchList(this.listQuery).then(response => {
        this.list = response.data.items
        this.total = response.data.total

        // Just to simulate the time of the request
        setTimeout(() => {
          this.listLoading = false
        }, 1.5 * 1000)
      })
    },
    handleFilter() {
      this.listQuery.page = 1
      this.getList()
    },
    handleModifyStatus(row, status) {
      this.$message({
        message: '操作Success',
        type: 'success'
      })
      row.status = status
    },
    sortChange(data) {
      const { prop, order } = data
      if (prop === 'id') {
        this.sortByID(order)
      }
    },
    sortByID(order) {
      if (order === 'ascending') {
        this.listQuery.sort = '+id'
      } else {
        this.listQuery.sort = '-id'
      }
      this.handleFilter()
    },
    resetTemp() {
      this.temp = {
        id: undefined,
        importance: 1,
        remark: '',
        timestamp: new Date(),
        title: '',
        status: 'published',
        type: ''
      }
    },
    handleCreate() {
      this.resetTemp()
      this.dialogStatus = 'create'
      this.dialogFormVisible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].clearValidate()
      })
    },
    createData() {
      this.$refs['dataForm'].validate((valid) => {
        if (valid) {
          this.temp.id = parseInt(Math.random() * 100) + 1024 // mock a id
          this.temp.author = 'vue-element-admin'
          createArticle(this.temp).then(() => {
            this.list.unshift(this.temp)
            this.dialogFormVisible = false
            this.$notify({
              title: '成功',
              message: '新建成功',
              type: 'success',
              duration: 2000
            })
          })
        }
      })
    },
    handleUpdate(row) {
      this.temp = Object.assign({}, row) // copy obj
      this.temp.timestamp = new Date(this.temp.timestamp)
      this.dialogStatus = 'update'
      this.dialogFormVisible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].clearValidate()
      })
    },
    updateData() {
      this.$refs['dataForm'].validate((valid) => {
        if (valid) {
          const tempData = Object.assign({}, this.temp)
          tempData.timestamp = +new Date(tempData.timestamp) // change Thu Nov 30 2017 16:41:05 GMT+0800 (CST) to 1512031311464
          updateArticle(tempData).then(() => {
            const index = this.list.findIndex(v => v.id === this.temp.id)
            this.list.splice(index, 1, this.temp)
            this.dialogFormVisible = false
            this.$notify({
              title: '成功',
              message: '修改成功',
              type: 'success',
              duration: 2000
            })
          })
        }
      })
    },
    handleDelete(row, index) {
      this.$notify({
        title: '成功',
        message: '删除成功',
        type: 'success',
        duration: 2000
      })
      this.list.splice(index, 1)
    },
    handleFetchPv(pv) {
      fetchPv(pv).then(response => {
        this.pvData = response.data.pvData
        this.dialogPvVisible = true
      })
    },
    handleDownload() {
      this.downloadLoading = true
      import('@/vendor/Export2Excel').then(excel => {
        const tHeader = ['timestamp', 'title', 'type', 'importance', 'status']
        const filterVal = ['timestamp', 'title', 'type', 'importance', 'status']
        const data = this.formatJson(filterVal)
        excel.export_json_to_excel({
          header: tHeader,
          data,
          filename: 'table-list'
        })
        this.downloadLoading = false
      })
    },
    formatJson(filterVal) {
      return this.list.map(v => filterVal.map(j => {
        if (j === 'timestamp') {
          return parseTime(v[j])
        } else {
          return v[j]
        }
      }))
    },
    getSortClass: function(key) {
      const sort = this.listQuery.sort
      return sort === `+${key}` ? 'ascending' : 'descending'
    }
  }
}
</script>
