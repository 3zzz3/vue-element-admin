<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input v-model="listQuery.liuShui" placeholder="流水号" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.wanGongDan" placeholder="完工单号" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.jiHua" placeholder="计划编号" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.zuoYe" placeholder="作业名称" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.ChanPing" placeholder="产品名称" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.jianYan" placeholder="检验批次" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.ShengChanDate" placeholder="生产日期" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.jianYanXiang" placeholder="检验项" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.jianYanJieGuo" placeholder="检验结果" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.jianYanRen" placeholder="检验人" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-input v-model="listQuery.jianYantime" placeholder="检验时间" style="width: 200px;" class="filter-item" @keyup.enter.native="handleFilter" />
      <el-button v-waves class="filter-item" type="primary" icon="el-icon-search" @click="handleFilter">
        搜索
      </el-button>
      <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="el-icon-edit" @click="handleCreate">
        新增
      </el-button>
      <el-button class="filter-item" style="margin-left: 10px;" type="primary" icon="el-icon-refresh">
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
      <el-table-column label="流水号" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.liuShui }}</span>
        </template>
      </el-table-column>
      <el-table-column label="计划编号" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.jiHua }}</span>
        </template>
      </el-table-column>
      <el-table-column label="完工单号" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.wanGongDan }}</span>
        </template>
      </el-table-column>
      <el-table-column label="作业名称" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.zuoYe }}</span>
        </template>
      </el-table-column>
      <el-table-column label="生产线名称" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.shengChanXian }}</span>
        </template>
      </el-table-column>
      <el-table-column label="检验批次" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.jianYan }}</span>
        </template>
      </el-table-column>
      <el-table-column label="产品名称" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.ChanPing }}</span>
        </template>
      </el-table-column>
      <el-table-column label="生产日期" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.ShengChanDate }}</span>
        </template>
      </el-table-column>
      <el-table-column label="检验项" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.jianYanXiang }}</span>
        </template>
      </el-table-column>
      <el-table-column label="生产数量" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.shengChanShu }}</span>
        </template>
      </el-table-column>
      <el-table-column label="抽检数量" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.chouJianShu }}</span>
        </template>
      </el-table-column>
      <el-table-column label="检验值" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.jianYanZhi }}</span>
        </template>
      </el-table-column>
      <el-table-column label="阈值上限" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.YuZhiShang }}</span>
        </template>
      </el-table-column>
      <el-table-column label="检验结果" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.jianYanJieGuo }}</span>
        </template>
      </el-table-column>
      <el-table-column label="检验人" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.jianYanRen }}</span>
        </template>
      </el-table-column>
      <el-table-column label="检验时间" prop="id" align="center" width="100">
        <template slot-scope="{row}">
          <span>{{ row.jianYantime }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" align="center" width="150" class-name="small-padding fixed-width">
        <template slot-scope="{row,$index}">
          <el-button type="primary" size="mini" @click="handleUpdate(row)">
            修改
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
        <el-form-item label="完工单号" prop="wanGongDan">
          <el-input v-model="temp.wanGongDan" />
        </el-form-item>
        <el-form-item label="检验标准" prop="jianYanBiaoZhun">
          <el-input v-model="temp.jianYanBiaoZhun" />
        </el-form-item>
        <el-form-item label="抽检数量" prop="chouJianShu">
          <el-input v-model="temp.chouJianShu" />
        </el-form-item>
        <el-form-item label="检验值" prop="jianYanZhi">
          <el-input v-model="temp.jianYanZhi" />
        </el-form-item>
        <el-form-item label="检验结果" prop="jianYanJieGuo">
          <el-input v-model="temp.jianYanJieGuo" />
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
        type: [{ required: true, message: 'type is required', trigger: 'change' }],
        timestamp: [{ type: 'date', required: true, message: 'timestamp is required', trigger: 'change' }],
        title: [{ required: true, message: 'title is required', trigger: 'blur' }]
      },
      downloadLoading: false
    }
  },
  created() {
    this.getList()
  },
  methods: {
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
