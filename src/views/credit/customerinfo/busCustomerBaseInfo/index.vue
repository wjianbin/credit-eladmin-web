<template>
  <div class="app-container">
    <!--工具栏-->
    <div class="head-container">
      <div v-if="crud.props.searchToggle">
        <!-- 搜索 -->
        <label class="el-form-item-label">老系统里面的客户编号</label>
        <el-input v-model="query.customerid" clearable placeholder="老系统里面的客户编号" style="width: 185px;" class="filter-item" @keyup.enter.native="crud.toQuery" />
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
          <el-form-item label="老系统里面的客户编号">
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
          <el-form-item label="信息报告日期">
            <el-input v-model="form.rptdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="报告时点说明代码">
            <el-select v-model="form.rptdatecode" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.RptDateCode"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="客户资料维护机构代码">
            <el-input v-model="form.cimoc" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="客户资料类型">
            <el-select v-model="form.customertype" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.CustomerType"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="性别">
            <el-select v-model="form.sex" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.Sex"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="出生日期">
            <el-input v-model="form.dob" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="国籍">
            <el-input v-model="form.nation" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="户籍地址">
            <el-input v-model="form.houseadd" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="户籍所在地行政区划">
            <el-input v-model="form.hhdist" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="手机号码">
            <el-input v-model="form.cellphone" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="电子邮箱">
            <el-input v-model="form.email" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息更新日期">
            <el-input v-model="form.fcsinfoupdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="婚姻状况">
            <el-select v-model="form.maristatus" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.MariStatus"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="配偶姓名">
            <el-input v-model="form.sponame" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="配偶证件类型">
            <el-input v-model="form.spoidtype" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="配偶证件号码">
            <el-input v-model="form.spoidnum" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="配偶联系电话">
            <el-input v-model="form.spotel" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="配偶工作单位">
            <el-input v-model="form.spscmpynm" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息更新日期">
            <el-input v-model="form.spsinfoupdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="学历">
            <el-select v-model="form.edulevel" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.EduLevel"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="学位">
            <el-select v-model="form.acadegree" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.AcaDegree"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="信息更新日期">
            <el-input v-model="form.eduinfoupdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="就业状况">
            <el-select v-model="form.empstatus" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.EmpStatus"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="单位名称">
            <el-input v-model="form.cpnname" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="单位性质">
            <el-select v-model="form.cpntype" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.CpnType"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="单位所属行业">
            <el-select v-model="form.industry" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.Industry"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="单位详细地址">
            <el-input v-model="form.cpnaddr" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="单位所在地邮编">
            <el-input v-model="form.cpnpc" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="单位所在地行政区划">
            <el-input v-model="form.cpndist" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="单位电话">
            <el-input v-model="form.cpntel" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="职业">
            <el-select v-model="form.occupation" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.Occupation"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="职务">
            <el-select v-model="form.title" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.Title"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="职称">
            <el-select v-model="form.techtitle" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.TechTitle"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="本单位工作起始年份">
            <el-input v-model="form.workstartdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息更新日期">
            <el-input v-model="form.octpninfoupdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="居住状况">
            <el-select v-model="form.resistatus" filterable placeholder="请选择">
              <el-option
                v-for="item in dict.ResiStatus"
                :key="item.id"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="居住地详细地址">
            <el-input v-model="form.resiaddr" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="居住地邮编">
            <el-input v-model="form.resipc" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="居住地行政区划">
            <el-input v-model="form.residist" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="住宅电话">
            <el-input v-model="form.hometel" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息更新日期">
            <el-input v-model="form.resiinfoupdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="通讯地址">
            <el-input v-model="form.mailaddr" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="通讯地邮编">
            <el-input v-model="form.mailpc" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="通讯地行政区划">
            <el-input v-model="form.maildist" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息更新日期">
            <el-input v-model="form.mlginfoupdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="自报年收入">
            <el-input v-model="form.annlinc" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="纳税年收入">
            <el-input v-model="form.taxincome" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="信息更新日期">
            <el-input v-model="form.incinfoupdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="创建时间">
            <el-input v-model="form.insertdate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="更新时间">
            <el-input v-model="form.updatedate" style="width: 370px;" />
          </el-form-item>
          <el-form-item label="上报时间">
            <el-input v-model="form.uploaddate" style="width: 370px;" />
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
        <el-table-column prop="customerid" label="老系统里面的客户编号" />
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
        <el-table-column prop="rptdate" label="信息报告日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.rptdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="rptdatecode" label="报告时点说明代码">
          <template slot-scope="scope">
            {{ dict.label.RptDateCode[scope.row.rptdatecode] }}
          </template>
        </el-table-column>
        <el-table-column prop="cimoc" label="客户资料维护机构代码" />
        <el-table-column prop="customertype" label="客户资料类型">
          <template slot-scope="scope">
            {{ dict.label.CustomerType[scope.row.customertype] }}
          </template>
        </el-table-column>
        <el-table-column prop="sex" label="性别">
          <template slot-scope="scope">
            {{ dict.label.Sex[scope.row.sex] }}
          </template>
        </el-table-column>
        <el-table-column prop="dob" label="出生日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.dob) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="nation" label="国籍" />
        <el-table-column prop="houseadd" label="户籍地址" />
        <el-table-column prop="hhdist" label="户籍所在地行政区划" />
        <el-table-column prop="cellphone" label="手机号码" />
        <el-table-column prop="email" label="电子邮箱" />
        <el-table-column prop="fcsinfoupdate" label="信息更新日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.fcsinfoupdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="maristatus" label="婚姻状况">
          <template slot-scope="scope">
            {{ dict.label.MariStatus[scope.row.maristatus] }}
          </template>
        </el-table-column>
        <el-table-column prop="sponame" label="配偶姓名" />
        <el-table-column prop="spoidtype" label="配偶证件类型">
          <template slot-scope="scope">
            {{ dict.label.IDType[scope.row.spoidtype] }}
          </template>
        </el-table-column>
        <el-table-column prop="spoidnum" label="配偶证件号码" />
        <el-table-column prop="spotel" label="配偶联系电话" />
        <el-table-column prop="spscmpynm" label="配偶工作单位" />
        <el-table-column prop="spsinfoupdate" label="信息更新日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.spsinfoupdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="edulevel" label="学历">
          <template slot-scope="scope">
            {{ dict.label.EduLevel[scope.row.edulevel] }}
          </template>
        </el-table-column>
        <el-table-column prop="acadegree" label="学位">
          <template slot-scope="scope">
            {{ dict.label.AcaDegree[scope.row.acadegree] }}
          </template>
        </el-table-column>
        <el-table-column prop="eduinfoupdate" label="信息更新日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.eduinfoupdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="empstatus" label="就业状况">
          <template slot-scope="scope">
            {{ dict.label.EmpStatus[scope.row.empstatus] }}
          </template>
        </el-table-column>
        <el-table-column prop="cpnname" label="单位名称" />
        <el-table-column prop="cpntype" label="单位性质">
          <template slot-scope="scope">
            {{ dict.label.CpnType[scope.row.cpntype] }}
          </template>
        </el-table-column>
        <el-table-column prop="industry" label="单位所属行业">
          <template slot-scope="scope">
            {{ dict.label.Industry[scope.row.industry] }}
          </template>
        </el-table-column>
        <el-table-column prop="cpnaddr" label="单位详细地址" />
        <el-table-column prop="cpnpc" label="单位所在地邮编" />
        <el-table-column prop="cpndist" label="单位所在地行政区划" />
        <el-table-column prop="cpntel" label="单位电话" />
        <el-table-column prop="occupation" label="职业">
          <template slot-scope="scope">
            {{ dict.label.Occupation[scope.row.occupation] }}
          </template>
        </el-table-column>
        <el-table-column prop="title" label="职务">
          <template slot-scope="scope">
            {{ dict.label.Title[scope.row.title] }}
          </template>
        </el-table-column>
        <el-table-column prop="techtitle" label="职称">
          <template slot-scope="scope">
            {{ dict.label.TechTitle[scope.row.techtitle] }}
          </template>
        </el-table-column>
        <el-table-column prop="workstartdate" label="本单位工作起始年份">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.workstartdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="octpninfoupdate" label="信息更新日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.octpninfoupdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="resistatus" label="居住状况">
          <template slot-scope="scope">
            {{ dict.label.ResiStatus[scope.row.resistatus] }}
          </template>
        </el-table-column>
        <el-table-column prop="resiaddr" label="居住地详细地址" />
        <el-table-column prop="resipc" label="居住地邮编" />
        <el-table-column prop="residist" label="居住地行政区划" />
        <el-table-column prop="hometel" label="住宅电话" />
        <el-table-column prop="resiinfoupdate" label="信息更新日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.resiinfoupdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="mailaddr" label="通讯地址" />
        <el-table-column prop="mailpc" label="通讯地邮编" />
        <el-table-column prop="maildist" label="通讯地行政区划" />
        <el-table-column prop="mlginfoupdate" label="信息更新日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.mlginfoupdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="annlinc" label="自报年收入" />
        <el-table-column prop="taxincome" label="纳税年收入" />
        <el-table-column prop="incinfoupdate" label="信息更新日期">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.incinfoupdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="insertdate" label="创建时间">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.insertdate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="updatedate" label="更新时间">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.updatedate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="uploaddate" label="上报时间">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.uploaddate) }}</span>
          </template>
        </el-table-column>
        <el-table-column prop="uploadstatus" label="上报状态" />
        <el-table-column prop="uploadflag" label="上报标识" />
        <el-table-column prop="createTime" label="create_time">
          <template slot-scope="scope">
            <span>{{ parseTime(scope.row.createTime) }}</span>
          </template>
        </el-table-column>
        <el-table-column v-permission="['admin','busCustomerBaseInfo:edit','busCustomerBaseInfo:del']" label="操作" width="150px" align="center">
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
import crudBusCustomerBaseInfo from '@/api/busCustomerBaseInfo'
import CRUD, { presenter, header, form, crud } from '@crud/crud'
import rrOperation from '@crud/RR.operation'
import crudOperation from '@crud/CRUD.operation2'
import udOperation from '@crud/UD.operation'
import pagination from '@crud/Pagination'

const defaultForm = { id: null, customerid: null, infrectype: null, name: null, idtype: null, idnum: null, infsurccode: null, rptdate: null, rptdatecode: null, cimoc: null, customertype: null, sex: null, dob: null, nation: null, houseadd: null, hhdist: null, cellphone: null, email: null, fcsinfoupdate: null, maristatus: null, sponame: null, spoidtype: null, spoidnum: null, spotel: null, spscmpynm: null, spsinfoupdate: null, edulevel: null, acadegree: null, eduinfoupdate: null, empstatus: null, cpnname: null, cpntype: null, industry: null, cpnaddr: null, cpnpc: null, cpndist: null, cpntel: null, occupation: null, title: null, techtitle: null, workstartdate: null, octpninfoupdate: null, resistatus: null, resiaddr: null, resipc: null, residist: null, hometel: null, resiinfoupdate: null, mailaddr: null, mailpc: null, maildist: null, mlginfoupdate: null, annlinc: null, taxincome: null, incinfoupdate: null, insertdate: null, updatedate: null, uploaddate: null, uploadstatus: null, uploadflag: null, createTime: null }
export default {
  name: 'BusCustomerBaseInfo',
  components: { pagination, crudOperation, rrOperation, udOperation },
  mixins: [presenter(), header(), form(defaultForm), crud()],
  dicts: ['InfRecType', 'IDType', 'RptDateCode', 'CustomerType', 'Sex', 'MariStatus', 'IDType', 'EduLevel', 'AcaDegree', 'EmpStatus', 'CpnType', 'Industry', 'Occupation', 'Title', 'TechTitle', 'ResiStatus'],
  cruds() {
    return CRUD({ title: '/api/customerBaseInfo', url: 'api/busCustomerBaseInfo', sort: 'id,desc', crudMethod: { ...crudBusCustomerBaseInfo }})
  },
  data() {
    return {
      permission: {
        add: ['admin', 'busCustomerBaseInfo:add'],
        edit: ['admin', 'busCustomerBaseInfo:edit'],
        del: ['admin', 'busCustomerBaseInfo:del']
      },
      rules: {
        id: [
          { required: true, message: 'id不能为空', trigger: 'blur' }
        ]
      },
      queryTypeOptions: [
        { key: 'customerid', display_name: '老系统里面的客户编号' }
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
