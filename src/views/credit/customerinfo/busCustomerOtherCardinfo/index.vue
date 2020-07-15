<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">customerId</label>
        <el-input v-model="query.customerid" clearable placeholder="customerId" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
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
          <el-form-item label="customerId">
            <el-input v-model="form.customerid" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="姓名">
            <el-input v-model="form.alias" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="证件类型">
            <el-select v-model="form.othidtype" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.IDType"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="证件号码">
            <el-input v-model="form.othidnum" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息更新日期">
            <el-input v-model="form.idinfoupdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="create_time">
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
        <el-table-column prop="customerid" label="customerId" />
        <el-table-column prop="alias" label="姓名" />
        <el-table-column prop="othidtype" label="证件类型">
          <template slot-scope="scope">
            {{ dict.label.IDType[scope.row.othidtype] }}
          </template>
        </el-table-column>
        <el-table-column prop="othidnum" label="证件号码" />
        <el-table-column prop="idinfoupdate" label="信息更新日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.idinfoupdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="createTime" label="create_time">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busCustomerOtherCardinfo:edit','busCustomerOtherCardinfo:del']" label="操作" width="150px" align="center">
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
import crudBusCustomerOtherCardinfo from '@/api/busCustomerOtherCardinfo'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation2'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, customerid: null, alias: null, othidtype: null, othidnum: null, idinfoupdate: null, createTime: null }
export default {
  name: 'BusCustomerOtherCardinfo',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['IDType'],
  cruds() {
    return CRUD({ title: '/api/customerothercardinfo', url: 'api/busCustomerOtherCardinfo', sort: 'id,desc', crudMethod: { ...crudBusCustomerOtherCardinfo }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busCustomerOtherCardinfo:add'],
        edit: ['admin', 'busCustomerOtherCardinfo:edit'],
        del: ['admin', 'busCustomerOtherCardinfo:del']
      },
      rules: {
        id: [
          { required: true, message: 'id不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'customerid', display_name: 'customerId' }
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
