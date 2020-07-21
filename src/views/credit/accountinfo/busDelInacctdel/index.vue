<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">借据编号</label>
        <el-input v-model="query.accloanid" clearable placeholder="借据编号" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">待删除业务标识码</label>
        <el-input v-model="query.delreccode" clearable placeholder="待删除业务标识码" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="借据编号">
            <el-input v-model="form.accloanid" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="待删除业务标识码">
            <el-input v-model="form.delreccode" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="待删除段段标">
            <el-select v-model="form.delsgmtcode" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.acct_duan_code"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="待删除起始日期">
            <el-input v-model="form.delstartdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="待删除结束日期">
            <el-input v-model="form.delenddate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="上报状态">
            <el-input v-model="form.uploadstatus" style="width: 370px;" />
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
        <el-table-column prop="accloanid" label="借据编号" />
        <el-table-column prop="infrectype" label="信息记录类型" />
        <el-table-column prop="delreccode" label="待删除业务标识码" />
        <el-table-column prop="delsgmtcode" label="待删除段段标">
          <template slot-scope="scope">
            {{ dict.label.acct_duan_code[scope.row.delsgmtcode] }}
          </template>
        </el-table-column>
        <el-table-column prop="delstartdate" label="待删除起始日期" />
        <el-table-column prop="delenddate" label="待删除结束日期" />
        <el-table-column prop="uploadstatus" label="上报状态" />
        <el-table-column prop="uploadflag" label="上报标识" />
        <el-table-column prop="createTime" label="create_time">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busDelInacctdel:edit','busDelInacctdel:del']" label="操作" width="150px" align="center">
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
import crudBusDelInacctdel from '@/api/acc/busDelInacctdel'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation2'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, accloanid: null, infrectype: null, delreccode: null, delsgmtcode: null, delstartdate: null, delenddate: null, uploadstatus: null, uploadflag: null, createTime: null }
export default {
  name: 'BusDelInacctdel',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['acct_duan_code'],
  cruds() {
    return CRUD({ title: '/api/del_inacctdel', url: 'api/busDelInacctdel', sort: 'id,desc', crudMethod: { ...crudBusDelInacctdel }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busDelInacctdel:add'],
        edit: ['admin', 'busDelInacctdel:edit'],
        del: ['admin', 'busDelInacctdel:del']
      },
      rules: {
      },
      queryTypeOptions: [
        { key: 'accloanid', display_name: '借据编号' },
        { key: 'delreccode', display_name: '待删除业务标识码' }
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
