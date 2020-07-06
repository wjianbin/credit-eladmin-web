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
                :value="item.value" />
            </el-select>
          </el-form-item>
          <el-form-item label="A姓名">
            <el-input v-model="form.name" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="A证件类型">
            <el-select v-model="form.idtype" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.IDType"
                :key="item.id"
                :label="item.label"
                :value="item.value" />
            </el-select>
          </el-form-item>
          <el-form-item label="A证件号码">
            <el-input v-model="form.idnum" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="B姓名">
            <el-input v-model="form.fammemname" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="B证件类型">
            <el-select v-model="form.fammemcerttype" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.IDType"
                :key="item.id"
                :label="item.label"
                :value="item.value" />
            </el-select>
          </el-form-item>
          <el-form-item label="B证件号码">
            <el-input v-model="form.fammemcertnum" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="家族关系">
            <el-select v-model="form.famrel" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.FamRel"
                :key="item.id"
                :label="item.label"
                :value="item.value" />
            </el-select>
          </el-form-item>
          <el-form-item label="家族关系有效标志">
            <el-select v-model="form.famrelaassflag" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.FamRelaAssFlag"
                :key="item.id"
                :label="item.label"
                :value="item.value" />
            </el-select>
          </el-form-item>
          <el-form-item label="信息来源编码">
            <el-input v-model="form.infsurccode" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息报告日期">
            <el-input v-model="form.rptdate" style="width: 370px;" />
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
        <el-table-column prop="customerid" label="客户编号" />
        <el-table-column prop="infrectype" label="信息记录类型">
          <template slot-scope="scope">
            {{ dict.label.InfRecType[scope.row.infrectype] }}
          </template>
        </el-table-column>
        <el-table-column prop="name" label="A姓名" />
        <el-table-column prop="idtype" label="A证件类型">
          <template slot-scope="scope">
            {{ dict.label.IDType[scope.row.idtype] }}
          </template>
        </el-table-column>
        <el-table-column prop="idnum" label="A证件号码" />
        <el-table-column prop="fammemname" label="B姓名" />
        <el-table-column prop="fammemcerttype" label="B证件类型">
          <template slot-scope="scope">
            {{ dict.label.IDType[scope.row.fammemcerttype] }}
          </template>
        </el-table-column>
        <el-table-column prop="fammemcertnum" label="B证件号码" />
        <el-table-column prop="famrel" label="家族关系">
          <template slot-scope="scope">
            {{ dict.label.FamRel[scope.row.famrel] }}
          </template>
        </el-table-column>
        <el-table-column prop="famrelaassflag" label="家族关系有效标志">
          <template slot-scope="scope">
            {{ dict.label.FamRelaAssFlag[scope.row.famrelaassflag] }}
          </template>
        </el-table-column>
        <el-table-column prop="infsurccode" label="信息来源编码" />
        <el-table-column prop="rptdate" label="信息报告日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.rptdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="uploadstatus" label="上报状态" />
        <el-table-column prop="uploadflag" label="上报标识" />
        <el-table-column prop="createTime" label="createTime">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busCustomerRelationship:edit','busCustomerRelationship:del']" label="操作" width="150px" align="center">
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
import crudBusCustomerRelationship from '@/api/busCustomerRelationship'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, customerid: null, infrectype: null, name: null, idtype: null, idnum: null, fammemname: null, fammemcerttype: null, fammemcertnum: null, famrel: null, famrelaassflag: null, infsurccode: null, rptdate: null, uploadstatus: null, uploadflag: null, createTime: null }
export default {
  name: 'BusCustomerRelationship',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['InfRecType', 'IDType', 'IDType', 'FamRel', 'FamRelaAssFlag'],
  cruds() {
    return CRUD({ title: '/api/customerrelationship', url: 'api/busCustomerRelationship', sort: 'id,desc', crudMethod: { ...crudBusCustomerRelationship }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busCustomerRelationship:add'],
        edit: ['admin', 'busCustomerRelationship:edit'],
        del: ['admin', 'busCustomerRelationship:del']
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
