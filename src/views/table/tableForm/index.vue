<template>
  <div class="app-container">
    <div class="engineer-container">
      <div class="engineer-header bg">
        <div class="hd-lf">{{project_Name_Code}}</div>
        <div class="hd-rt">
          <el-button type="primary" size="small">保存</el-button>
          <el-button type="primary" size="small">提交</el-button>
          <el-button type="primary" size="small">返回</el-button>
        </div>
      </div>
      <div class="engineer-body">
        <div class="table-hd">
          <ul class="table-txt bg">
            <li class="fee0 feeType">费用类型</li>
            <li class="fee1 feeGiv">送审</li>
            <li class="fee1 feeFir">初审</li>
            <li class="fee1 feeAud">审定</li>
            <li class="fee2 feeSub">审减</li>
            <li class="fee2 feeRate">审减率</li>
          </ul>
        </div>
        <div class="table-bd" >
          <!-- ad3 -->
          <div class="ad3" v-for="(item,index) in audit3" :key="item.auditIdentify">
            <ul class="ad3-tit bg">
              <li class="fee1">施工单位：{{item.auditName}}</li>
              <li class="fee1">订单名称：{{item.itemCompanyrefDetail.length == 0 ? "" : item.itemCompanyrefDetail[0].orderName}}</li>
              <li class="fee1">订单编号：{{item.itemCompanyrefDetail.length == 0 ? "" : item.itemCompanyrefDetail[0].orderId}}</li>
              <li class="fee1">
                单项工程名称：
                <el-input v-model="item.projName"></el-input>
              </li>
              <li class="fee2">
                <el-button type="primary" size="small">{{item.flag=='10'?'通信':'土建'}}</el-button>
              </li>
              <li v-if="item.flag=='20'" class="fee1">
                建筑面积：
                <el-input v-model="item.constructionArea"></el-input>m²
              </li>
            </ul>
            <ul class="ad3-total">
              <li class="fee0">
                <span>
                  {{item.flag=='10'?'建安费用（施工费）合计':'工程造价'}}
                </span>
                <!-- <a class="btn-ins" @click="addMore(item)" v-if="idx=='0'"><img src="@/assets/img/add.png" /></a> -->
                <a class="btn-ins" @click="addMore(item)"><img :src="imgs.addIcon" /></a>
              </li>
              <!-- <li></li> -->
              <li class="fee1"><el-input v-model="item.giveMoney" disabled></el-input></li>
              <li class="fee1"><el-input v-model="item.firstMoney" disabled></el-input></li>
              <li class="fee1">
                <el-input v-model="item.auditMoney" disabled></el-input>
              </li>
              <li class="fee2">{{!isValueNull(item.auditsubMoney)?'--':item.auditsubMoney}}</li>
              <li class="fee2">{{!isValueNull(item.auditsubRate)?'--':item.auditsubRate}}</li>
            </ul>
            <ul class="ad3-detail " v-for="(it,idx) in item.itemCompanyrefDetail" :key="idx">
              <li class="fee0" v-if="it.detailId!=''&&it.expenseTypeName!=''" style="padding-left:20px;">{{it.expenseTypeName}}</li>
              <li class="fee0" v-if="it.detailId==''" style="padding-left:20px;">
                <el-input v-model="it.expenseTypeName" placeholder="其中：自定义" style="width:82%"></el-input>
                <a class="btn-ins" @click="delMore(item,idx)"><img :src="imgs.delIcon" /></a>
              </li>
              <li class="fee1"><el-input v-model="it.giveMoney" :disabled="it.detailId!=''" ></el-input></li>
              <li class="fee1"><el-input v-model="it.firstMoney" :disabled="it.detailId!=''" @blur=minusDetail3(it.expenseTypeName,it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input></li>
              <li class="fee1">
                <el-input v-model="it.auditMoney" @blur=minusDetail3(it.expenseTypeName,it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input>
              </li>
              <li class="fee2">{{!isValueNull(it.auditsubMoney)?'--':it.auditsubMoney}}</li>
              <li class="fee2">{{!isValueNull(it.auditsubRate)?'--':it.auditsubRate}}</li>
            </ul>
            <ul class="ad3-jgc ">
              <li class="fee0">建安费用（甲供材料费）</li>
              <!-- <li></li> -->
              <li class="fee1"><el-input v-model="item.giveMoneyFp" disabled></el-input></li>
              <li class="fee1"><el-input v-model="item.firstMoneyFp" disabled></el-input></li>
              <li class="fee1">
                <el-input v-model="item.auditMoneyFp" @blur=minus3Fp(item.firstMoneyFp,item.auditMoneyFp,index)></el-input>
              </li>
              <li class="fee2">{{!isValueNull(item.auditsubMoneyFp)?'--':item.auditsubMoneyFp}}</li>
              <li class="fee2">{{!isValueNull(item.auditsubRateFp)?'--':item.auditsubRateFp}}</li>
            </ul>
          </div>
          <!-- ad1 设计费-->
          <div class="ad1" v-for="(item,index) in audit1" :key="item.auditIdentify">
            <ul class="ad3-tit bg">
              <li class="fee1">施工单位：{{item.auditName}}</li>
              <li class="fee1">订单名称：{{item.itemCompanyrefDetail.length == 0 ? "" : item.itemCompanyrefDetail[0].orderName}}</li>
              <li class="fee1">订单编号：{{item.itemCompanyrefDetail.length == 0 ? "" : item.itemCompanyrefDetail[0].orderId}}</li>
              <li class="fee1">
                单项工程名称：
                <el-input v-model="item.projName"></el-input>
              </li>
            </ul>
            <ul class="ad3-total">
              <!-- <li class="fee0">{{item.expenseTypeName}}</li> -->
              <li class="fee0">
                <span>
                  设计费**
                </span>
                <a class="btn-ins" @click="addMore(item)"><img :src="imgs.addIcon" /></a>
              </li>
              <li class="fee1"><el-input v-model="item.giveMoney" disabled></el-input></li>
              <li class="fee1"><el-input v-model="item.firstMoney" disabled></el-input></li>
              <li class="fee1">
                <el-input v-model="item.auditMoney" disabled></el-input>
              </li>
              <li class="fee2">{{!isValueNull(item.auditsubMoney)?'--':item.auditsubMoney}}</li>
              <li class="fee2">{{!isValueNull(item.auditsubRate)?'--':item.auditsubRate}}</li>
            </ul>
            <ul class="ad3-detail " v-for="(it,idx) in item.itemCompanyrefDetail" :key="idx">
              <!-- <li class="fee0">{{it.expenseTypeName}}</li> -->
              <li class="fee0" v-if="it.detailId!=''&&it.expenseTypeName!=''">
                <span>
                  其中：{{it.expenseTypeName}}
                </span>
              </li>
              <li class="fee0" v-if="it.detailId==''">
                <el-input v-model="it.expenseTypeName" placeholder="其中：自定义" style="width:70%"></el-input>
                <a class="btn-ins" @click="delMore(item,idx)"><img :src="imgs.delIcon" /></a>
              </li>
              <li class="fee1"><el-input v-model="it.giveMoney" :disabled="it.detailId!=''" @blur=minusDetail1(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input></li>
              <li class="fee1"><el-input v-model="it.firstMoney" :disabled="it.detailId!=''" @blur=minusDetail1(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input></li>
              <li class="fee1">
                <el-input v-model="it.auditMoney" @blur=minusDetail1(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input>
              </li>
              <li class="fee2">{{!isValueNull(it.auditsubMoney)?'--':it.auditsubMoney}}</li>
              <li class="fee2">{{!isValueNull(it.auditsubRate)?'--':it.auditsubRate}}</li>
            </ul>
          </div>
          <!-- ad2监理费 -->
          <div class="ad2" v-for="(item,index) in audit2" :key="item.auditIdentify">
            <ul class="ad3-tit bg">
              <li class="fee1">施工单位：{{item.auditName}}</li>
              <li class="fee1">订单名称：{{item.itemCompanyrefDetail.length == 0 ? "" : item.itemCompanyrefDetail[0].orderName}}</li>
              <li class="fee1">订单编号：{{item.itemCompanyrefDetail.length == 0 ? "" : item.itemCompanyrefDetail[0].orderId}}</li>
              <li class="fee1">
                单项工程名称：
                <el-input v-model="item.projName"></el-input>
              </li>
            </ul>
            <ul class="ad3-total">
               <li class="fee0">
                <span>
                  监理费**
                </span>
                <a class="btn-ins" @click="addMore(item)"><img :src="imgs.addIcon" /></a>
              </li>
              <li class="fee1" ><el-input v-model="item.giveMoney" disabled></el-input></li>
              <li class="fee1" ><el-input v-model="item.firstMoney" disabled></el-input></li>
              <li class="fee1" >
                <el-input v-model="item.auditMoney" disabled></el-input>
              </li>
              <li class="fee2" >{{!isValueNull(item.auditsubMoney)?'--':item.auditsubMoney}}</li>
              <li class="fee2" >{{!isValueNull(item.auditsubRate)?'--':item.auditsubRate}}</li>
            </ul>
            <ul class="ad3-detail " v-for="(it,idx) in item.itemCompanyrefDetail" :key="idx">
              <li class="fee0" v-if="it.detailId!=''&&it.expenseTypeName!=''">
                <span>
                  其中：{{it.expenseTypeName}}
                </span>
              </li>
              <li class="fee0" v-if="it.detailId==''">
                <el-input v-model="it.expenseTypeName" placeholder="其中：自定义" style="width:70%"></el-input>
                <a class="btn-ins" @click="delMore(item,idx)"><img :src="imgs.delIcon" /></a>
              </li>
              <li class="fee1"><el-input v-model="it.giveMoney" :disabled="it.detailId!=''" @blur=minusDetail2(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input></li>
              <li class="fee1"><el-input v-model="it.firstMoney" :disabled="it.detailId!=''" @blur=minusDetail2(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input></li>
              <li class="fee1">
                <el-input v-model="it.auditMoney" @blur=minusDetail2(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input>
              </li>
              <li class="fee2">{{!isValueNull(it.auditsubMoney)?'--':it.auditsubMoney}}</li>
              <li class="fee2">{{!isValueNull(it.auditsubRate)?'--':it.auditsubRate}}</li>
            </ul>
          </div>
          <!-- ad14 -->
          <div class="ad4" v-for="(item,index) in audit4" :key="item.auditIdentify">
            <ul class="ad3-tit bg">
              <li class="fee1">施工单位：{{item.auditName}}</li>
              <li class="fee1">订单名称：{{item.itemCompanyrefDetail.length == 0 ? "" : item.itemCompanyrefDetail[0].orderName}}</li>
              <li class="fee1">订单编号：{{item.itemCompanyrefDetail.length == 0 ? "" : item.itemCompanyrefDetail[0].orderId}}</li>
              <li class="fee1">
                单项工程名称：
                <el-input v-model="item.projName"></el-input>
              </li>
            </ul>
            <ul class="ad3-total">
              <li class="fee0">{{item.expenseTypeName}}</li>
              <!-- <li></li> -->
              <li class="fee1"><el-input v-model="item.giveMoney" disabled></el-input></li>
              <li class="fee1"><el-input v-model="item.firstMoney" disabled></el-input></li>
              <li class="fee1">
                <el-input v-model="item.auditMoney" disabled></el-input>
              </li>
              <li class="fee2">{{!isValueNull(item.auditsubMoney)?'--':item.auditsubMoney}}</li>
              <li class="fee2">{{!isValueNull(item.auditsubRate)?'--':item.auditsubRate}}</li>
            </ul>
            <ul class="ad3-detail " v-for="(it,idx) in item.itemCompanyrefDetail" :key="idx">
              <li class="fee0">{{it.expenseTypeName}}</li>
              <li class="fee1"><el-input v-model="it.giveMoney" :disabled="it.detailId!=''" @blur=minusDetail4(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input></li>
              <li class="fee1"><el-input v-model="it.firstMoney" :disabled="it.detailId!=''" @blur=minusDetail4(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input></li>
              <li class="fee1">
                <el-input v-model="it.auditMoney" @blur=minusDetail4(it.giveMoney,it.firstMoney,it.auditMoney,index,idx)></el-input>
              </li>
              <li class="fee2">{{!isValueNull(it.auditsubMoney)?'--':it.auditsubMoney}}</li>
              <li class="fee2">{{!isValueNull(it.auditsubRate)?'--':it.auditsubRate}}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'TableForm',
  data() {
    return {
      imgs: {
        addIcon: require('@/assets/img/add.png'),
        delIcon: require('@/assets/img/delete.png')
      },
      audit3: [],
      audit1: [],
      audit2: [],
      audit4: [],
      project_Name_Code: '',
      metaData: [],
      gm: {
            inputUser: null,
            updateUser: null,
            inputStamp: "2023-12-12 08:09:55",
            updateStamp: "2023-12-12 08:09:55",
            giveitemId: 3236111,
            itemId: "21733",
            cpmisId: null,
            itemCode: "结算A",
            itemName: "结算A1",
            planType: null,
            specialSmallType: "",
            typeId: 0,
            manageLevel: null,
            company: "0099002274",
            companyName: "中国电信股份有限公司智慧家庭运营中心",
            department: null,
            departmentName: null,
            itemSource: "2",
            itemType: "01",
            itemUrl: null,
            auditemStatus: "05",
            projectState: "1",
            processinstid: null,
            currentlink: "承接",
            currentState: "1",
            currentDate: null,
            auditMethod: "1",
            audUnit: "32",
            audUnitName: "中国电信江苏公司",
            giveMoney: "90.00",
            firstMoney: "90.00",
            auditMoney: null,
            returnBackDate: null,
            returnBackReason: null,
            itemPrincipal: "王宇",
            itemPrincipalId: "32031170@JS",
            itemPrincipalUser: null,
            submitTime: "2023-12-12",
            appDate: "2023-12-12",
            auditDate: null,
            closeDate: null,
            pigeonholeDate: null,
            giveNum: null,
            giveAuditTotal: null,
            giveAuditNum: null,
            assignUserId: null,
            assignUserName: null,
            projectArranger: "32031170@JS",
            projectArrangerName: null,
            projectArrangerPhone: null,
            orgMethod: "01",
            itemAuditType: "10",
            ifEntrust: "0",
            outUnitCode: "",
            outUnitName: "",
            outUnitPerson: "",
            outHtbh: null,
            auditInformNo: null,
            outPrice: null,
            designreplycode: null,
            designreplysum: null,
            designreplydate: null,
            itemmanId: "1966740",
            itemmanName: "王宇",
            itemman: null,
            startworkdate: null,
            firstacceptdate: null,
            lastacceptdate: null,
            processLog: null,
            itemBasicInfoModel: null,
            giveItemContentModels: [
              {
                inputUser: null,
                updateUser: null,
                inputStamp: "2023-12-12 08:09:55",
                updateStamp: "2023-12-12 08:09:55",
                giveItemId: 3236111,
                itemCode: "结算A",
                cpmisId: null,
                auditCode: "G100014722",
                auditName: "0112测试供应商",
                auditType: "3",
                giveMoney: "30.00",
                firstMoney: "30.00",
                auditMoney: null,
                giveMoneyBackup: "30.00",
                firstMoneyBackup: "30.00",
                auditIdentify: "SGXZ34835",
                giveMoneyFp: "10.00",
                firstMoneyFp: "10.00",
                auditMoneyFp: "0",
                giveMoneyFpBackup: "10.00",
                firstMoneyFpBackup: "10.00",
                bedgetid: null,
                entrustmentCode: null,
                entrustmentContent: null,
                framworkCode: null,
                sgdwname: null,
                itemCompanyrefDetail: [
                  {
                    inputUser: null,
                    updateUser: null,
                    inputStamp: "2023-12-12 08:09:55",
                    updateStamp: "2023-12-12 08:09:55",
                    giveItemId: 3236111,
                    companyId: "G100014722",
                    expenseType: "05",
                    expenseTypeName: "施工费（不含乙供材和安全生产费）",
                    orderId: null,
                    orderName: null,
                    contractCode: null,
                    contractName: null,
                    giveAuditFee: "10.00",
                    firstMoney: "10.00",
                    auditMoney: null,
                    constructIdentify: null,
                    auditIdentify: "SGXZ34835",
                    giveAuditFee1: "10.00",
                    firstMoney1: "10.00",
                    giveMoneyFp: "10.00",
                    firstMoneyFp: "10.00",
                    giveMoneyFpBackup: "10.00",
                    firstMoneyFpBackup: "10.00",
                    detailId: "01HHEF20DQ185QJ48Y86YBRJNH",
                    lineItemId: null
                  },
                  {
                    inputUser: null,
                    updateUser: null,
                    inputStamp: "2023-12-12 08:09:55",
                    updateStamp: "2023-12-12 08:09:55",
                    giveItemId: 3236111,
                    companyId: "G100014722",
                    expenseType: "08",
                    expenseTypeName: "乙供材料费",
                    orderId: null,
                    orderName: null,
                    contractCode: null,
                    contractName: null,
                    giveAuditFee: "10.00",
                    firstMoney: "10.00",
                    auditMoney: null,
                    constructIdentify: null,
                    auditIdentify: "SGXZ34835",
                    giveAuditFee1: "10.00",
                    firstMoney1: "10.00",
                    giveMoneyFp: null,
                    firstMoneyFp: null,
                    giveMoneyFpBackup: null,
                    firstMoneyFpBackup: null,
                    detailId: "01HHEF20DQZ100Y09HJ1JCKZ4H",
                    lineItemId: null
                  },
                  {
                    inputUser: null,
                    updateUser: null,
                    inputStamp: "2023-12-12 08:09:55",
                    updateStamp: "2023-12-12 08:09:55",
                    giveItemId: 3236111,
                    companyId: "G100014722",
                    expenseType: "09",
                    expenseTypeName: "安全生产费",
                    orderId: null,
                    orderName: null,
                    contractCode: null,
                    contractName: null,
                    giveAuditFee: "10.00",
                    firstMoney: "10.00",
                    auditMoney: null,
                    constructIdentify: null,
                    auditIdentify: "SGXZ34835",
                    giveAuditFee1: "10.00",
                    firstMoney1: "10.00",
                    giveMoneyFp: null,
                    firstMoneyFp: null,
                    giveMoneyFpBackup: null,
                    firstMoneyFpBackup: null,
                    detailId: "01HHEF20DQ6M7GGNKHB835FVMH",
                    lineItemId: null
                  }
                ],
                giveNumber: null,
                projName: null,
                projCode: null,
                contentId: null
              },
              {
                inputUser: null,
                updateUser: null,
                inputStamp: "2023-12-12 08:09:55",
                updateStamp: "2023-12-12 08:09:55",
                giveItemId: 3236111,
                itemCode: "结算A",
                cpmisId: null,
                auditCode: "G100012066",
                auditName: "dd0722",
                auditType: "2",
                giveMoney: "20.00",
                firstMoney: "20.00",
                auditMoney: null,
                giveMoneyBackup: "20.00",
                firstMoneyBackup: "20.00",
                auditIdentify: "SGXZ34834",
                giveMoneyFp: "0.00",
                firstMoneyFp: "0.00",
                auditMoneyFp: "0",
                giveMoneyFpBackup: "0.00",
                firstMoneyFpBackup: "0.00",
                bedgetid: null,
                entrustmentCode: null,
                entrustmentContent: null,
                framworkCode: null,
                sgdwname: null,
                itemCompanyrefDetail: [
                  {
                    inputUser: null,
                    updateUser: null,
                    inputStamp: "2023-12-12 08:09:55",
                    updateStamp: "2023-12-12 08:09:55",
                    giveItemId: 3236111,
                    companyId: "G100012066",
                    expenseType: "02",
                    expenseTypeName: "监理费",
                    orderId: null,
                    orderName: null,
                    contractCode: null,
                    contractName: null,
                    giveAuditFee: "10.00",
                    firstMoney: "10.00",
                    auditMoney: null,
                    constructIdentify: null,
                    auditIdentify: "SGXZ34834",
                    giveAuditFee1: "10.00",
                    firstMoney1: "10.00",
                    giveMoneyFp: "0.00",
                    firstMoneyFp: "0.00",
                    giveMoneyFpBackup: "0.00",
                    firstMoneyFpBackup: "0.00",
                    detailId: "01HHEF20DJEWXFQDA7YZZ07VXM",
                    lineItemId: null
                  },
                  {
                    inputUser: null,
                    updateUser: null,
                    inputStamp: "2023-12-12 08:09:55",
                    updateStamp: "2023-12-12 08:09:55",
                    giveItemId: 3236112,
                    companyId: "G100012066",
                    expenseType: "02",
                    expenseTypeName: "监理费3333",
                    orderId: null,
                    orderName: null,
                    contractCode: null,
                    contractName: null,
                    giveAuditFee: "10.00",
                    firstMoney: "10.00",
                    auditMoney: null,
                    constructIdentify: null,
                    auditIdentify: "SGXZ34834",
                    giveAuditFee1: "10.00",
                    firstMoney1: "10.00",
                    giveMoneyFp: "0.00",
                    firstMoneyFp: "0.00",
                    giveMoneyFpBackup: "0.00",
                    firstMoneyFpBackup: "0.00",
                    detailId: "01HHEF20DJEWXFQDA7YZZ07VTM",
                    lineItemId: null
                  }
                ],
                giveNumber: null,
                projName: null,
                projCode: null,
                contentId: null
              },
              {
                inputUser: null,
                updateUser: null,
                inputStamp: "2023-12-12 08:09:55",
                updateStamp: "2023-12-12 08:09:55",
                giveItemId: 3236111,
                itemCode: "结算A",
                cpmisId: null,
                auditCode: "G100012059",
                auditName: "特殊单位供应商",
                auditType: "1",
                giveMoney: "10.00",
                firstMoney: "10.00",
                auditMoney: null,
                giveMoneyBackup: "10.00",
                firstMoneyBackup: "10.00",
                auditIdentify: "SGXZ34833",
                giveMoneyFp: "0.00",
                firstMoneyFp: "0.00",
                auditMoneyFp: "0",
                giveMoneyFpBackup: "0.00",
                firstMoneyFpBackup: "0.00",
                bedgetid: null,
                entrustmentCode: null,
                entrustmentContent: null,
                framworkCode: null,
                sgdwname: null,
                itemCompanyrefDetail: [
                  {
                    inputUser: null,
                    updateUser: null,
                    inputStamp: "2023-12-12 08:09:55",
                    updateStamp: "2023-12-12 08:09:55",
                    giveItemId: 3236111,
                    companyId: "G100012059",
                    expenseType: "01",
                    expenseTypeName: "设计费",
                    orderId: null,
                    orderName: null,
                    contractCode: null,
                    contractName: null,
                    giveAuditFee: "10.00",
                    firstMoney: "10.00",
                    auditMoney: null,
                    constructIdentify: null,
                    auditIdentify: "SGXZ34833",
                    giveAuditFee1: "10.00",
                    firstMoney1: "10.00",
                    giveMoneyFp: "0.00",
                    firstMoneyFp: "0.00",
                    giveMoneyFpBackup: "0.00",
                    firstMoneyFpBackup: "0.00",
                    detailId: "01HHEF20DEH8M875YBKV2B183N",
                    lineItemId: null
                  }
                ],
                giveNumber: null,
                projName: null,
                projCode: null,
                contentId: null
              },
              {
                inputUser: null,
                updateUser: null,
                inputStamp: "2023-12-12 08:09:55",
                updateStamp: "2023-12-12 08:09:55",
                giveItemId: 3236111,
                itemCode: "结算A",
                cpmisId: null,
                auditCode: "G100014655",
                auditName: "XX有限公司26162727-2",
                auditType: "1",
                giveMoney: "40.00",
                firstMoney: "40.00",
                auditMoney: null,
                giveMoneyBackup: "40.00",
                firstMoneyBackup: "40.00",
                auditIdentify: "SGXZ34832",
                giveMoneyFp: "0.00",
                firstMoneyFp: "0.00",
                auditMoneyFp: "0",
                giveMoneyFpBackup: "0.00",
                firstMoneyFpBackup: "0.00",
                bedgetid: null,
                entrustmentCode: null,
                entrustmentContent: null,
                framworkCode: null,
                sgdwname: null,
                itemCompanyrefDetail: [
                  {
                    inputUser: null,
                    updateUser: null,
                    inputStamp: "2023-12-12 08:09:55",
                    updateStamp: "2023-12-12 08:09:55",
                    giveItemId: 3236111,
                    companyId: "G100014655",
                    expenseType: "01",
                    expenseTypeName: "设计费",
                    orderId: null,
                    orderName: null,
                    contractCode: null,
                    contractName: null,
                    giveAuditFee: "40.00",
                    firstMoney: "40.00",
                    auditMoney: null,
                    constructIdentify: null,
                    auditIdentify: "SGXZ34832",
                    giveAuditFee1: "40.00",
                    firstMoney1: "40.00",
                    giveMoneyFp: "0.00",
                    firstMoneyFp: "0.00",
                    giveMoneyFpBackup: "0.00",
                    firstMoneyFpBackup: "0.00",
                    detailId: "01HHEF20DBQZXTJVQWAD71GZ16",
                    lineItemId: null
                  }
                ],
                giveNumber: null,
                projName: null,
                projCode: null,
                contentId: null
              },
              {
                inputUser: null,
                updateUser: null,
                inputStamp: "2023-12-12 08:09:55",
                updateStamp: "2023-12-12 08:09:55",
                giveItemId: 3236111,
                itemCode: "结算A",
                cpmisId: null,
                auditCode: "11111111",
                auditName: "广州闻知工程造价咨询有限公司",
                auditType: "3",
                giveMoney: "20.00",
                firstMoney: "20.00",
                auditMoney: null,
                giveMoneyBackup: "20.00",
                firstMoneyBackup: "20.00",
                auditIdentify: "SGXZ34831",
                giveMoneyFp: "30.00",
                firstMoneyFp: "30.00",
                auditMoneyFp: "0",
                giveMoneyFpBackup: "30.00",
                firstMoneyFpBackup: "30.00",
                bedgetid: null,
                entrustmentCode: null,
                entrustmentContent: null,
                framworkCode: null,
                sgdwname: null,
                itemCompanyrefDetail: [
                  {
                    inputUser: null,
                    updateUser: null,
                    inputStamp: "2023-12-12 08:09:55",
                    updateStamp: "2023-12-12 08:09:55",
                    giveItemId: 3236111,
                    companyId: "11111111",
                    expenseType: "03",
                    expenseTypeName: "施工费（含乙供材和安全生产费）",
                    orderId: null,
                    orderName: null,
                    contractCode: null,
                    contractName: null,
                    giveAuditFee: "20.00",
                    firstMoney: "20.00",
                    auditMoney: null,
                    constructIdentify: null,
                    auditIdentify: "SGXZ34831",
                    giveAuditFee1: "20.00",
                    firstMoney1: "20.00",
                    giveMoneyFp: "30.00",
                    firstMoneyFp: "30.00",
                    giveMoneyFpBackup: "30.00",
                    firstMoneyFpBackup: "30.00",
                    detailId: "01HHEF20D7KN6T7MKXYGC61T2W",
                    lineItemId: null
                  }
                ],
                giveNumber: null,
                projName: null,
                projCode: null,
                contentId: null
              }
            ],
            itemOthersModels: null,
            attachmentInfos: null,
            itemVerdictModels: null,
            itemVerdictPrintModels: null,
            accountModel: null,
            audittypeCode: "0701",
            projectId: "23-JS-0701-000169",
            typeCodeFk: null,
            giveMoneyBackup: null,
            firstMoneyBackup: null,
            designreplycodeBackup: null,
            designreplysumBackup: null,
            workloadIn: null,
            workloadOut: null,
            designreplydateBackup: null,
            serialNumber: null,
            reportFile: null,
            serialNumberOut: null,
            reportFileOut: null,
            outTaxRate: null,
            outTax: null,
            outTaxPrice: null,
            sampleType: null,
            itemTypeSam: null,
            sampleState: null,
            projectPersonId: "32031170@JS",
            projectPersonName: "王宇",
            isRepeat: null,
            isBedget: null,
            siteAuditFlag: null,
            outHtName: null,
            itemVerdictFirstTime: null,
            investmentChannels: null,
            isTax: null,
            isCost: null,
            warningInfoList: null,
            materialCode: null,
            materialName: null,
            presentName: null,
            presentId: null,
            transferPerson: null,
            accumulatedAmount: null
        },
        formType:''// 判断初始化项目是通信还是土建
    }
  },
  created() {
    this.$nextTick(()=> {
      this.project_Name_Code=this.gm.itemCode+'('+this.gm.itemName+')';
      this.formType=this.gm.itemAuditType;
      this.gm.giveItemContentModels.forEach(item=>{
        if(item.auditType == "1"){
          this.audit1.push(item)
        }else if(item.auditType == "2"){
          this.audit2.push(item)
        }else if(item.auditType == "3"){
          this.audit3.push(item)
        }else if(item.auditType == "4"){
          this.audit4.push(item)
        }
        if(item.auditMoney == null){
          item.auditMoney='0.00'
        }
      })

      // 数据处理--audit3
      console.log(this.audit3,'created--3',this.audit2)
      this.dealAudit3();
      this.dealAuditOther();
    })
  },
  methods: {
    isValueNull(val){
      if (val!=undefined&&val!='undefined'&&val!=null&&val!='null'&&val!='') {
        return true
      } else{
        return false
      }
    },
    dealAudit3() {
      this.$nextTick(()=> {
        if(this.audit3.length != 0){
          this.audit3.forEach((item,index)=>{
            console.log(item,'7777')
            item['showDetail']=true;
            item['showDedu']=false;
            item['remark']="";
            item['constructionArea']="";
            item['showExplantanation']=false;
            if(this.isValueNull(item.auditMoneyFp)) {
              if (item.auditMoneyFp.length=='1') {
                item.auditMoneyFp=Number(item.auditMoneyFp).toFixed(2);
              }
              console.log(item.auditMoneyFp.length,'length',item.auditMoneyFp)

            }else{
              item.auditMoneyFp='0.00'
            }
            if (this.formType=='10') {
              item['flag']='10';
            }else{
              item['flag']='20'; 
            }
            if (item.flag=='20'){
              item['deduArr']=[];
            }
            if(item.itemCompanyrefDetail.length != 0){
                let obj= {
                  expenseTypeName:"甲供材料费扣款",
                  detailId: "jgckk",
                  giveMoney: "0.00",
                  firstMoney: "0.00",
                  auditMoney: "0.00",
                  isInput: "jgckk",
                  isEdit: "1",
                  auditIdentify: item.auditIdentify||""
                }
              if (item.flag=="10") {
              
                item.itemCompanyrefDetail.push(obj);
              }
              item.itemCompanyrefDetail.forEach((itm,idx)=>{
                if (itm.auditMoney == null) {
                  itm.auditMoney='0.00'
                }
                itm.auditMoney = itm.auditMoney
                if (itm.giveAuditFee) {
                  itm.giveMoney=itm.giveAuditFee
                }
              })
            }
          })
          let arr1=[];
          for(let a=0;a<this.audit3.length;a++) {
            if (this.audit3[a].flag=='20') {
              let obj1={
                expenseTypeName:"应扣甲供材料费",
                companyId:this.audit3[a].auditCode,
                giveAuditFee:"0.00",
                firstMoney:"0.00",
                auditMoney:"0.00",
                isEdit:"1",
                orderId:"",
                contractCode:"",
                orderName:"",
                constructIdentify:"",
                auditIdentify:this.audit3[a].auditIdentify,
                detailId:""
              };
              arr1.push(obj1)
              let obj2={
                expenseTypeName:"应扣水电费",
                companyId:this.audit3[a].auditCode,
                giveAuditFee:"0.00",
                firstMoney:"0.00",
                auditMoney:"0.00",
                isEdit:"1",
                orderId:"",
                contractCode:"",
                orderName:"",
                constructIdentify:"",
                auditIdentify:this.audit3[a].auditIdentify,
                detailId:""
              };
              arr1.push(obj2)
              let obj3={
                expenseTypeName:"应扣其他款项",
                companyId:this.audit3[a].auditCode,
                giveAuditFee:"0.00",
                firstMoney:"0.00",
                auditMoney:"0.00",
                isEdit:"1",
                orderId:"",
                contractCode:"",
                orderName:"",
                constructIdentify:"",
                auditIdentify:this.audit3[a].auditIdentify,
                detailId:""
              };
              arr1.push(obj3)
            }
          }
          arr1.map(it1=> {
            this.audit3.map(it2=> {
              if (it2.auditIdentify==it1.auditIdentify&&it2.flag=='20') {
                it2.deduArr.push(it1) 
              }
            })
          })
          this.audit3.map((item,index)=> {
            this.minus3Fp(item.firstMoneyFp,item.auditMoneyFp,index);
            if(item.itemCompanyrefDetail.length != 0){
              item.itemCompanyrefDetail.map((itm,idx)=> {
                this.minusDetail3(itm.expenseTypeName,itm.giveAuditFee,itm.firstMoney,itm.auditMoney,index,idx)
              })
            }
            // 土建审定金额
            if (item.flag=='20'&&item.deduArr.length!=0) {
              item.deduArr.map((itm1,idx1)=> {
                this.minusDedu(itm1.giveAuditFee,itm1.firstMoney,itm1.auditMoney,idx1,index)
              })
            }
          })
        }
      })
    },
    dealAuditOther() {
      this.$nextTick(()=> {
        if(this.audit1.length != 0){
          this.audit1.forEach(item=>{
            item['remark']="";
            item['showExplantanation']=false;
            if(item.itemCompanyrefDetail.length != 0){
              item.itemCompanyrefDetail.forEach(itm=>{
                if(itm.auditMoney == null){
                  itm.auditMoney='0.00'
                }
                itm.auditMoney = itm.auditMoney
                itm.giveMoney=itm.giveAuditFee;
              })
            }
          })
          this.audit1.map((item,index)=> {
            if(item.itemCompanyrefDetail.length != 0){
              item.itemCompanyrefDetail.map((itm,idx)=> {
                this.minusDetail1(itm.giveAuditFee,itm.firstMoney,itm.auditMoney,index,idx)
              })
            }
          })
        }
        if(this.audit2.length != 0){
          this.audit2.forEach(item=>{
            item['remark']="";
            item['showExplantanation']=false;
            if(item.itemCompanyrefDetail.length != 0){
              item.itemCompanyrefDetail.forEach(itm=>{
                if(itm.auditMoney == null){
                  itm.auditMoney='0.00'
                }
                itm.auditMoney = itm.auditMoney
                itm.giveMoney=itm.giveAuditFee;
              })
            }
          })
          this.audit2.map((item,index)=> {
            if(item.itemCompanyrefDetail.length != 0){
              item.itemCompanyrefDetail.map((itm,idx)=> {
                this.minusDetail2(itm.giveAuditFee,itm.firstMoney,itm.auditMoney,index,idx)
              })
            }
          })
        }
        if(this.audit4.length != 0){
          this.audit4.forEach(item=>{
            item['remark']="";
            item['showExplantanation']=false;
            if(item.itemCompanyrefDetail.length != 0){
              item.itemCompanyrefDetail.forEach(itm=>{
                if(itm.auditMoney == null){
                  itm.auditMoney='0.00'
                }
                itm.auditMoney = itm.auditMoney
                itm.giveMoney=itm.giveAuditFee;
              })
            }
          })
          this.audit4.map((item,index)=> {
            if(item.itemCompanyrefDetail.length != 0){
              item.itemCompanyrefDetail.map((itm,idx)=> {
                this.minusDetail4(itm.giveAuditFee,itm.firstMoney,itm.auditMoney,index,idx)
              })
            }
          })
        }
      })
    },
    // 建安费用（施工费）合计
    minus3(data1,data2,data3,index){
      if (this.isValueNull(data2)) {
        // let count = new Decimal(Number(data2)).sub(new Decimal(Number(data3))).toNumber();
        let count=Number(data2)-Number(data3)
        this.$set(this.audit3[index],"auditsubMoney",count.toFixed(2));
        // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
        let percent=(count/Number(data2)*100)
        percent = percent || 0
        let percentStr =parseFloat(data2)==0?'':(percent.toFixed(2) + "%")
        this.$set(this.audit3[index],"auditsubRate",percentStr)
        // this.sgfTotal();
      }
    },
    // 建安费用（甲供材料费）
    minus3Fp(data1,data2,index){
      if (this.isValueNull(data2)) {
        // let count = new Decimal(Number(data1)).sub(new Decimal(Number(data2))).toNumber();
        let count=Number(data1)-Number(data2)
        this.$set(this.audit3[index],"auditsubMoneyFp",count.toFixed(2));
        // let percent = new Decimal(Number(count)).div(new Decimal(data1)).toNumber() * 100;
        let percent=(count/Number(data1)*100)
        percent = percent || 0
        let percentStr = parseFloat(data1)==0?'':( percent.toFixed(2) + "%")
        this.$set(this.audit3[index],"auditsubRateFp",percentStr)
      }
    },
    // 建安费每一项
    minusDetail3(expenseTypeName,data1,data2,data3,index,idx){
      console.log(expenseTypeName,data1,data2,data3,idx,'minusDetail3')
      let label = expenseTypeName
      let reg = new RegExp('甲供材料费扣款')
      if (reg.test(label)) {
        // 为甲供材扣款
        // if (this.isValueNull(data3)) {
          // let count = new Decimal(this.isValueNull(data3)?data3:0).sub(new Decimal(data2)).toNumber()
          let count = (Number(data2)-Number(data3))
          this.$set(this.audit3[index].itemCompanyrefDetail[idx],"auditsubMoney",count.toFixed(2))
          // let percent = new Decimal(Number(count)).div(new Decimal(this.isValueNull(data2)?data2:0)).toNumber() * 100;
          let percent = (count/Number(data2)*100)
          percent = percent || 0
          let percentStr =parseFloat(data2)==0?'':(percent.toFixed(2) + "%") 
          this.$set(this.audit3[index].itemCompanyrefDetail[idx],"auditsubRate",percentStr)
          // 建安费统计送审总和
          let all0 = 0;
          let patt=new RegExp('甲供材料费扣款');
          this.audit3[index].itemCompanyrefDetail.forEach(item=>{
            if(item.giveMoney){
              all0 += Number(patt.test(item.expenseTypeName)?(-item.giveMoney):item.giveMoney)
            }
          })
          // 建安费统计初审总和
          let all1 = 0
          this.audit3[index].itemCompanyrefDetail.forEach(item=>{
            if(item.firstMoney){
              all1 += Number(patt.test(item.expenseTypeName)?(-item.firstMoney):item.firstMoney)
            }
          })
          // 建安费统计审定总和
          let all = 0
          this.audit3[index].itemCompanyrefDetail.forEach(item=>{
            if(item.auditMoney){
              all += Number(patt.test(item.expenseTypeName)?(-item.auditMoney):item.auditMoney)
            }
          })
          this.$set(this.audit3[index],"giveMoney",all0.toFixed(2));
          this.$set(this.audit3[index],"firstMoney",all1.toFixed(2));
          this.$set(this.audit3[index],"auditMoney",all.toFixed(2));
          this.minus3(this.audit3[index].giveMoney,this.audit3[index].firstMoney,this.audit3[index].auditMoney,index);
        // }
      }else{
        // 非甲供材扣款
        if (this.isValueNull(data2)) {
          // let count = new Decimal(data2).sub(new Decimal(this.isValueNull(data3)?data3:0)).toNumber()
          let count = (Number(data2)-Number(data3))
          this.$set(this.audit3[index].itemCompanyrefDetail[idx],"auditsubMoney",count.toFixed(2))
          // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
          let percent = (count/Number(data2)*100)
          percent = percent || 0
          let percentStr =parseFloat(data2)==0?'':(percent.toFixed(2) + "%") 
          this.$set(this.audit3[index].itemCompanyrefDetail[idx],"auditsubRate",percentStr)
          // 建安费统计送审总和
          let patt=new RegExp('甲供材料费扣款');
          let all0 = 0
          this.audit3[index].itemCompanyrefDetail.forEach(item=>{
            if(item.giveAuditFee){
              all0 += Number(patt.test(item.giveAuditFee)?(-item.giveAuditFee):item.giveAuditFee)
            }
          })
          // 建安费统计初审总和
          let all1 = 0
          this.audit3[index].itemCompanyrefDetail.forEach(item=>{
            if(item.firstMoney){
              all1 += Number(patt.test(item.expenseTypeName)?(-item.firstMoney):item.firstMoney)
            }
          })
          // 建安费统计审定总和
          let all = 0
          this.audit3[index].itemCompanyrefDetail.forEach(item=>{
            if(item.auditMoney){
              all += Number(patt.test(item.expenseTypeName)?(-item.auditMoney):item.auditMoney)
            }
          })
          this.$set(this.audit3[index],"giveMoney",all0.toFixed(2));
          this.$set(this.audit3[index],"firstMoney",all1.toFixed(2));
          this.$set(this.audit3[index],"auditMoney",all.toFixed(2));
          this.minus3(this.audit3[index].giveMoney,this.audit3[index].firstMoney,this.audit3[index].auditMoney,index);
        }
      }
      this.$forceUpdate();
    },

    // 土建--工程造价
    minusDedu(data1,data2,data3,ind,index) {
      console.log(data1,data2,data3,'minusDedu',ind,index)
      if (this.isValueNull(data2)&&this.isValueNull(data3)) {
        // let count = new Decimal(data2).sub(new Decimal(data3)).toNumber()
        let count = Number(data2)-Number(data3)
        this.$set(this.audit3[index].deduArr[ind],"auditsubMoney",count.toFixed(2))
        // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
        let percent = (count/Number(data2)*100)
        percent = percent || 0
        let percentStr =parseFloat(data2)==0?'':(percent.toFixed(2) + "%") 
        this.$set(this.audit3[index].deduArr[ind],"auditsubRate",percentStr)
        
        // this.sgfTotal();
      }
    },
    // 设计单位费用统计
    minus1(data1,data2,data3,index){
      // let count = new Decimal(Number(data2)).sub(new Decimal(Number(data3))).toNumber();
      let count=Number(data2)-Number(data3)
      this.$set(this.audit1[index],"auditsubMoney",count.toFixed(2));
      // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
      let percent=(count/Number(data2)*100)
      percent = percent || 0
      let percentStr = parseFloat(data2)==0?'':(percent.toFixed(2) + "%")
      this.$set(this.audit1[index],"auditsubRate",percentStr)
      // this.otherFee(this.audit1,this.audit2,this.audit4)
    },
    // 设计单位新增开支
    minusDetail1(data1,data2,data3,index,idx){
      // let count = new Decimal(data2).sub(new Decimal(this.isValueNull(data3)?data3:0)).toNumber()
      let count=Number(data2)-Number(data3)
      this.$set(this.audit1[index].itemCompanyrefDetail[idx],"auditsubMoney",count.toFixed(2))
      // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
      let percent=(count/Number(data2)*100)
      percent = percent || 0
      let percentStr = parseFloat(data2)==0?'':(percent.toFixed(2) + "%")
      this.$set(this.audit1[index].itemCompanyrefDetail[idx],"auditsubRate",percentStr)
      // 设计费统计送审总和
      let all0 = 0
      this.audit1[index].itemCompanyrefDetail.forEach(item=>{
        if(item.giveMoney){
          all0 += Number(item.giveMoney)
        }
      })
      // 设计费统计初审总和
      let all1 = 0
      this.audit1[index].itemCompanyrefDetail.forEach(item=>{
        if(item.firstMoney){
          all1 += Number(item.firstMoney)
        }
      })
      // 设计费统计审定总和
      let all = 0
      this.audit1[index].itemCompanyrefDetail.forEach(item=>{
        if(item.auditMoney){
          all += Number(item.auditMoney)
        }
      })
      this.$set(this.audit1[index],"giveMoney",all0.toFixed(2));
      this.$set(this.audit1[index],"firstMoney",all1.toFixed(2));
      this.$set(this.audit1[index],"auditMoney",all.toFixed(2));
      this.minus1(this.audit1[index].giveMoney,this.audit1[index].firstMoney,this.audit1[index].auditMoney,index);
    },
    // 监理单位费用统计
    minus2(data1,data2,data3,index){
      // if (this.isValueNull(data2)) {
        // let count = new Decimal(Number(data2)).sub(new Decimal(Number(data3))).toNumber();
        let count=Number(data2)-Number(data3)
        this.$set(this.audit2[index],"auditsubMoney",count.toFixed(2));
        // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
        let percent=(count/Number(data2)*100)
        percent = percent || 0
        let percentStr = parseFloat(data2)==0?'':(percent.toFixed(2) + "%")
        this.$set(this.audit2[index],"auditsubRate",percentStr)
        // this.otherFee(this.audit1,this.audit2,this.audit4)
      // }
    },
    // 监理单位新增开支 data1--sf,data2--cs,data3--sd, count--sj
    minusDetail2(data1,data2,data3,index,idx){
      if (this.isValueNull(data2)) {
        // let count = new Decimal(data2).sub(new Decimal(this.isValueNull(data3)?data3:0)).toNumber()
        let count=Number(data2)-Number(data3)
        this.$set(this.audit2[index].itemCompanyrefDetail[idx],"auditsubMoney",count.toFixed(2))
        // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
        let percent=(count/Number(data2)*100)
        percent = percent || 0
        let percentStr = parseFloat(data2)==0?'':(percent.toFixed(2) + "%")
        this.$set(this.audit2[index].itemCompanyrefDetail[idx],"auditsubRate",percentStr)
        // 监理费统计送审总和
        let all0 = 0
        this.audit2[index].itemCompanyrefDetail.forEach(item=>{
          if(item.giveMoney){
            all0 += Number(item.giveMoney)
          }
        })
        // 监理费统计初审总和
        let all1 = 0
        this.audit2[index].itemCompanyrefDetail.forEach(item=>{
          if(item.firstMoney){
            all1 += Number(item.firstMoney)
          }
        })
        // 监理费统计审定总和
        let all = 0
        this.audit2[index].itemCompanyrefDetail.forEach(item=>{
          if(item.auditMoney){
            all += Number(item.auditMoney)
          }
        })
        this.$set(this.audit2[index],"giveMoney",all0.toFixed(2));
        this.$set(this.audit2[index],"firstMoney",all1.toFixed(2));
        this.$set(this.audit2[index],"auditMoney",all.toFixed(2));
        this.minus2(this.audit2[index].giveMoney,this.audit2[index].firstMoney,this.audit2[index].auditMoney,index);
      }
    },
    // 集成单位合计
    minus4(data1,data2,data3,index){
      if (this.isValueNull(data2)) {
        let count=Number(data2)-Number(data3)
        // let count = new Decimal(Number(data2)).sub(new Decimal(Number(data3))).toNumber();
        this.$set(this.audit4[index],"auditsubMoney",count.toFixed(2));
        // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
        let percent=(count/Number(data2)*100)
        percent = percent || 0
        let percentStr = parseFloat(data2)==0?'':(percent.toFixed(2) + "%")
        this.$set(this.audit4[index],"auditsubRate",percentStr)
        // this.otherFee(this.audit1,this.audit2,this.audit4)
      }
    },
    // 集成单位新增
    minusDetail4(data1,data2,data3,index,idx){
      if (this.isValueNull(data2)) {
        // let count = new Decimal(data2).sub(new Decimal(this.isValueNull(data3)?data3:0)).toNumber()
        let count=Number(data2)-Number(data3)
        this.$set(this.audit4[index].itemCompanyrefDetail[idx],"auditsubMoney",count.toFixed(2))
        // let percent = new Decimal(Number(count)).div(new Decimal(data2)).toNumber() * 100;
        let percent=(count/Number(data2)*100)
        percent = percent || 0
        let percentStr = parseFloat(data2)==0?'':(percent.toFixed(2) + "%")
        this.$set(this.audit4[index].itemCompanyrefDetail[idx],"auditsubRate",percentStr)
        // 集成费统计送审总和
        let all0 = 0
        this.audit4[index].itemCompanyrefDetail.forEach(item=>{
          if(item.giveMoney){
            all0 += Number(item.giveMoney)
          }
        })
        // 集成费统计初审总和
        let all1 = 0
        this.audit4[index].itemCompanyrefDetail.forEach(item=>{
          if(item.firstMoney){
            all1 += Number(item.firstMoney)
          }
        })
        // 集成费统计审定总和
        let all = 0
        this.audit4[index].itemCompanyrefDetail.forEach(item=>{
          if(item.auditMoney){
            all += Number(item.auditMoney)
          }
        })
        this.$set(this.audit4[index],"giveMoney",all0.toFixed(2));
        this.$set(this.audit4[index],"firstMoney",all1.toFixed(2));
        this.$set(this.audit4[index],"auditMoney",all.toFixed(2));
        this.minus4(this.audit4[index].giveMoney,this.audit4[index].firstMoney,this.audit4[index].auditMoney,index);
      }
    },
    // 添加子项
    addMore(item){
      let obj = {
        detailId:"",
        isEdit: "1",// 新增时isEdit传1 非新增则是0
        expenseTypeName:"",
        giveMoney: "0.00",
        firstMoney: "0.00",
        auditMoney:'0.00',
        auditIdentify: item.auditIdentify||""
      }
      console.log(item,'addMore')
      item.itemCompanyrefDetail.push(obj)
    },
    // 删除子项
    delMore(item,index){
      console.log(item,'delMore',index)
      item.itemCompanyrefDetail.splice(index,1)
    },
  }
}
</script>
<style lang="scss" scoped>
ul,ul li {
  list-style-type: none;
}
.fee0 {
  width:30%;
}
.fee1 {
  width:20%;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
  .el-input {
    width:120px;
  }
}
.fee2 {
  width:10%;
}
.engineer-container {
  border:1px solid red;
  .bg {
    background:#ccc;
    height:40px;
    line-height:40px;
    border:none;
  }
  .engineer-header {
    display:flex;
    align-items:center;
    .hd-lf {
      width:50%;
    }
    .hd-rt {
      width:50%;
      text-align:right;
    }
  }
  .engineer-body {
    .table-hd {
      border:1px solid red;
      .table-txt {
        display:flex;
      }
    }
    .table-bd {
      border:1px solid green;
      .ad3-tit,.ad3-total,.ad3-detail,.ad3-jgc {
        display: flex;
        align-items:center;
      }
    }
  }
}
</style>
