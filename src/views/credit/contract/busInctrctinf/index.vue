<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">授信协议编号</label>
        <el-input v-model="query.inctrctinfid" clearable placeholder="授信协议编号" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">账户编号</label>
        <el-input v-model="query.inaccinfid" clearable placeholder="账户编号" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="授信协议编号" prop="inctrctinfid">
            <el-input v-model="form.inctrctinfid" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="账户编号" prop="inaccinfid">
            <el-input v-model="form.inaccinfid" style="width: 370px;" />
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="text" @click="crud.cancelCU">取消</el-button>
          <el-button :loading="crud.cu === 2" type="primary" @click="crud.submitCU">确认</el-button>
        </div>
      </el-dialog>
      <!--表格渲染-->
      <el-table ref="table" v-loading="crud.loading" :data="crud.data" size="small" style="width: 100%;" @selection-change="crud.selectionChangeHandler">
        <el-table-column type="selection" width="55" />
        <el-table-column prop="id" label="编号" />
        <el-table-column prop="inctrctinfid" label="授信协议编号" />
        <el-table-column prop="status" label="状态" />
        <el-table-column prop="inaccinfid" label="账户编号" />
        <el-table-column prop="uploadtime" label="上传时间">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.uploadtime) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="createtime" label="创建时间">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createtime) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="updatetime" label="更新时间">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.updatetime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busInctrctinf:edit','busInctrctinf:del']" label="操作" width="150px" align="center">
          <template slot-scope="scope">
            <udOperation
              :data="scope.row"
              :permission="permission"
            />
          </template>
        </el-table-column>
      </el-table>
      <!--分页组件-->
      <pagination />
    </div>
  </div>
</template>

<script>
import crudBusInctrctinf from '@/api/contract/busInctrctinf'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation2'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, inctrctinfid: null, status: null, inaccinfid: null, uploadtime: null, createtime: null, updatetime: null }
export default {
  name: 'BusInctrctinf',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: '/api/inctrctinf', url: 'api/busInctrctinf', sort: 'id,desc', crudMethod: { ...crudBusInctrctinf }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busInctrctinf:add'],
        edit: ['admin', 'busInctrctinf:edit'],
        del: ['admin', 'busInctrctinf:del']
      },
      rules: {
        inctrctinfid: [
          { required: true, message: '授信协议编号不能为空', trigger: 'blur' }
        ],
        inaccinfid: [
          { required: true, message: '账户编号不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'inctrctinfid', display_name: '授信协议编号' },
        { key: 'inaccinfid', display_name: '账户编号' }
      ]
    }
  },
  methods: {
    // 钩子：在获取表格数据之前执行，false 则代表不获取数据
    [CRUD.HOOK.beforeRefresh]() {
      return true
    }
  }
}
</script>

<style scoped>

</style>
