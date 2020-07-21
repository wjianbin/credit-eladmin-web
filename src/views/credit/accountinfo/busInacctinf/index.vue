<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">借据编号</label>
        <el-input v-model="query.accloanid" clearable placeholder="借据编号" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="id" prop="id">
            <el-input v-model="form.id" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="借据编号" prop="accloanid">
            <el-input v-model="form.accloanid" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="状态">
            <el-input v-model="form.status" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="创建时间">
            <el-input v-model="form.createtime" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="更新时间">
            <el-input v-model="form.updatetime" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="上报时间">
            <el-input v-model="form.uploadtime" style="width: 370px;" />
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
        <el-table-column prop="id" label="id" />
        <el-table-column prop="accloanid" label="借据编号" />
        <el-table-column prop="status" label="状态" />
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
        <el-table-column prop="uploadtime" label="上报时间">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.uploadtime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busInacctinf:edit','busInacctinf:del']" label="操作" width="150px" align="center">
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
import crudBusInacctinf from '@/api/acc/busInacctinf'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation2'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, accloanid: null, status: null, createtime: null, updatetime: null, uploadtime: null }
export default {
  name: 'BusInacctinf',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: '/api/inacctinf', url: 'api/busInacctinf', sort: 'id,desc', crudMethod: { ...crudBusInacctinf }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busInacctinf:add'],
        edit: ['admin', 'busInacctinf:edit'],
        del: ['admin', 'busInacctinf:del']
      },
      rules: {
        id: [
          { required: true, message: 'id不能为空', trigger: 'blur' }
        ],
        accloanid: [
          { required: true, message: '借据编号不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'accloanid', display_name: '借据编号' }
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
