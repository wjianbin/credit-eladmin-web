<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">任务名称</label>
        <el-input v-model="query.taskname" clearable placeholder="任务名称" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <label class="el-form-item-label">任务名称描述</label>
        <el-input v-model="query.taskdescribe" clearable placeholder="任务名称描述" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
        <rrOperation :crud="crud" />
      </div>
      <!--如果想在工具栏加入更多按钮，可以使用插槽方式， slot = 'left' or 'right'-->
      <crudOperation :permission="permission" />
      <!--表单组件-->
      <el-dialog :close-on-click-modal="false" :before-close="crud.cancelCU" :visible.sync="crud.status.cu > 0" :title="crud.status.title" width="500px">
        <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
          <el-form-item label="id">
            <el-input v-model="form.id" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="任务执行日期">
            <el-input v-model="form.batchdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="实际日期">
            <el-input v-model="form.businessdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="任务名称">
            <el-input v-model="form.taskname" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="任务名称描述">
            <el-input v-model="form.taskdescribe" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="任务所属模块">
            <el-input v-model="form.targetname" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="任务所属模块描述">
            <el-input v-model="form.targetdescribe" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="开始执行时间">
            <el-input v-model="form.begintime" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="执行结束时间">
            <el-input v-model="form.endtime" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="状态">
            <el-select v-model="form.status" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.TaskStatus"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
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
        <el-table-column prop="batchdate" label="任务执行日期" />
        <el-table-column prop="businessdate" label="实际日期" />
        <el-table-column prop="taskname" label="任务名称" />
        <el-table-column prop="taskdescribe" label="任务名称描述" />
        <el-table-column prop="targetname" label="任务所属模块" />
        <el-table-column prop="targetdescribe" label="任务所属模块描述" />
        <el-table-column prop="begintime" label="开始执行时间" />
        <el-table-column prop="endtime" label="执行结束时间" />
        <el-table-column prop="status" label="状态">
          <template slot-scope="scope">
            {{ dict.label.TaskStatus[scope.row.status] }}
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busTaskLog:edit','busTaskLog:del']" label="操作" width="150px" align="center">
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
import crudBusTaskLog from '@/api/busTaskLog'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, batchdate: null, businessdate: null, taskname: null, taskdescribe: null, targetname: null, targetdescribe: null, begintime: null, endtime: null, status: null }
export default {
  name: 'BusTaskLog',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['TaskStatus'],
  cruds() {
    return CRUD({ title: '/api/bus_task_log', url: 'api/busTaskLog', sort: 'id,desc', crudMethod: { ...crudBusTaskLog }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busTaskLog:add'],
        edit: ['admin', 'busTaskLog:edit'],
        del: ['admin', 'busTaskLog:del']
      },
      rules: {
      },
      queryTypeOptions: [
        { key: 'taskname', display_name: '任务名称' },
        { key: 'taskdescribe', display_name: '任务名称描述' }
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
