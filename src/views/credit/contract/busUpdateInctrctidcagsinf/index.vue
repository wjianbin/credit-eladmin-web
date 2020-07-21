<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">授信协议编号</label>
        <el-input v-model="query.inctrctinfid" clearable placeholder="授信协议编号" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="授信协议编号">
            <el-input v-model="form.inctrctinfid" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="原业务标识码">
            <el-input v-model="form.odbnescode" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="新业务标识码">
            <el-input v-model="form.nwbnescode" style="width: 370px;" />
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
        <el-table-column prop="inctrctinfid" label="授信协议编号" />
        <el-table-column prop="infrectype" label="信息记录类型">
          <template slot-scope="scope">
            {{ dict.label.acc_InfRecType[scope.row.infrectype] }}
          </template>
        </el-table-column>
        <el-table-column prop="odbnescode" label="原业务标识码" />
        <el-table-column prop="nwbnescode" label="新业务标识码" />
        <el-table-column prop="uploadstatus" label="上报状态" />
        <el-table-column prop="uploadflag" label="上报标识" />
        <el-table-column prop="createTime" label="createTime">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busUpdateInctrctidcagsinf:edit','busUpdateInctrctidcagsinf:del']" label="操作" width="150px" align="center">
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
import crudBusUpdateInctrctidcagsinf from '@/api/contract/busUpdateInctrctidcagsinf'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation2'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, inctrctinfid: null, infrectype: null, odbnescode: null, nwbnescode: null, uploadstatus: null, uploadflag: null, createTime: null }
export default {
  name: 'BusUpdateInctrctidcagsinf',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['acc_InfRecType'],
  cruds() {
    return CRUD({ title: '/api/update_inctrctidcagsinf', url: 'api/busUpdateInctrctidcagsinf', sort: 'id,desc', crudMethod: { ...crudBusUpdateInctrctidcagsinf }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busUpdateInctrctidcagsinf:add'],
        edit: ['admin', 'busUpdateInctrctidcagsinf:edit'],
        del: ['admin', 'busUpdateInctrctidcagsinf:del']
      },
      rules: {
      },
      queryTypeOptions: [
        { key: 'inctrctinfid', display_name: '授信协议编号' }
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
