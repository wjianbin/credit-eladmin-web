<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="id" prop="id">
            <el-input v-model="form.id" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="授信协议编号">
            <el-input v-model="form.inctrctinfid" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="对应业务操作的主键">
            <el-input v-model="form.busId" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息记录类型">
            <el-input v-model="form.infrectype" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="授信协议标识码">
            <el-input v-model="form.contractcode" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息报告日期">
            <el-input v-model="form.rptdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="报告时点说明代码">
            <el-input v-model="form.rptdatecode" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="姓名">
            <el-input v-model="form.name" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="证件类型">
            <el-input v-model="form.idtype" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="证件号码">
            <el-input v-model="form.idnum" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="业务管理机构代码">
            <el-input v-model="form.mngmtorgcode" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="上报状态">
            <el-input v-model="form.uploadstatus" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="上报标识">
            <el-input v-model="form.uploadflag" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="createTime">
            <el-input v-model="form.createTime" style="width: 370px;" />
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
        <el-table-column prop="busId" label="对应业务操作的主键" />
        <el-table-column prop="infrectype" label="信息记录类型" />
        <el-table-column prop="contractcode" label="授信协议标识码" />
        <el-table-column prop="rptdate" label="信息报告日期" />
        <el-table-column prop="rptdatecode" label="报告时点说明代码" />
        <el-table-column prop="name" label="姓名" />
        <el-table-column prop="idtype" label="证件类型" />
        <el-table-column prop="idnum" label="证件号码" />
        <el-table-column prop="mngmtorgcode" label="业务管理机构代码" />
        <el-table-column prop="uploadstatus" label="上报状态" />
        <el-table-column prop="uploadflag" label="上报标识" />
        <el-table-column prop="createTime" label="createTime">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busInctrctinfCtrctbssgmt:edit','busInctrctinfCtrctbssgmt:del']" label="操作" width="150px" align="center">
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
import crudBusInctrctinfCtrctbssgmt from '@/api/contract/busInctrctinfCtrctbssgmt'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, inctrctinfid: null, busId: null, infrectype: null, contractcode: null, rptdate: null, rptdatecode: null, name: null, idtype: null, idnum: null, mngmtorgcode: null, uploadstatus: null, uploadflag: null, createTime: null }
export default {
  name: 'BusInctrctinfCtrctbssgmt',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  cruds() {
    return CRUD({ title: '/api/inctrctinf_ctrctbssgmt', url: 'api/busInctrctinfCtrctbssgmt', sort: 'id,desc', crudMethod: { ...crudBusInctrctinfCtrctbssgmt }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busInctrctinfCtrctbssgmt:add'],
        edit: ['admin', 'busInctrctinfCtrctbssgmt:edit'],
        del: ['admin', 'busInctrctinfCtrctbssgmt:del']
      },
      rules: {
        id: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ]
      }}
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
