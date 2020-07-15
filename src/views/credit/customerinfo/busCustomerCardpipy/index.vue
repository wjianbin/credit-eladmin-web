<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">客户编号</label>
        <el-input v-model="query.customerid" clearable placeholder="客户编号" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
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
          <el-form-item label="客户编号">
            <el-input v-model="form.customerid" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息记录类型">
            <el-select v-model="form.infrectype" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.InfRecType"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="姓名">
            <el-input v-model="form.name" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="证件类型">
            <el-select v-model="form.idtype" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.IDType"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="证件号码">
            <el-input v-model="form.idnum" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息来源编码">
            <el-input v-model="form.infsurccode" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="证件有效期起始日期">
            <el-input v-model="form.idefctdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="证件有效期到期日期">
            <el-input v-model="form.idduedate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="证件签发机关名称">
            <el-input v-model="form.idorgname" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="֤证件签发机关所在地行政区划">
            <el-input v-model="form.iddist" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="客户资料维护机构代码">
            <el-input v-model="form.cimoc" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息报告日期">
            <el-input v-model="form.rptdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="上报状态（已上报，未上报）">
            <el-input v-model="form.uploaddate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="上报标识">
            <el-input v-model="form.uploadflag" style="width: 370px;" />
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
        <el-table-column prop="customerid" label="客户编号" />
        <el-table-column prop="infrectype" label="信息记录类型">
          <template slot-scope="scope">
            {{ dict.label.InfRecType[scope.row.infrectype] }}
          </template>
        </el-table-column>
        <el-table-column prop="name" label="姓名" />
        <el-table-column prop="idtype" label="证件类型">
          <template slot-scope="scope">
            {{ dict.label.IDType[scope.row.idtype] }}
          </template>
        </el-table-column>
        <el-table-column prop="idnum" label="证件号码" />
        <el-table-column prop="infsurccode" label="信息来源编码" />
        <el-table-column prop="idefctdate" label="证件有效期起始日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.idefctdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="idduedate" label="证件有效期到期日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.idduedate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="idorgname" label="证件签发机关名称" />
        <el-table-column prop="iddist" label="֤证件签发机关所在地行政区划" />
        <el-table-column prop="cimoc" label="客户资料维护机构代码" />
        <el-table-column prop="rptdate" label="信息报告日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.rptdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="uploaddate" label="上报状态（已上报，未上报）" />
        <el-table-column prop="uploadflag" label="上报标识" />
        <el-table-column prop="createTime" label="create_time">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busCustomerCardpipy:edit','busCustomerCardpipy:del']" label="操作" width="150px" align="center">
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
import crudBusCustomerCardpipy from '@/api/busCustomerCardpipy'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation2'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, customerid: null, infrectype: null, name: null, idtype: null, idnum: null, infsurccode: null, idefctdate: null, idduedate: null, idorgname: null, iddist: null, cimoc: null, rptdate: null, uploaddate: null, uploadflag: null, createTime: null }
export default {
  name: 'BusCustomerCardpipy',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['InfRecType', 'IDType'],
  cruds() {
    return CRUD({ title: '/api/customercardpipy', url: 'api/busCustomerCardpipy', sort: 'id,desc', crudMethod: { ...crudBusCustomerCardpipy }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busCustomerCardpipy:add'],
        edit: ['admin', 'busCustomerCardpipy:edit'],
        del: ['admin', 'busCustomerCardpipy:del']
      },
      rules: {
        id: [
          { required: true, message: 'id不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'customerid', display_name: '客户编号' }
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
