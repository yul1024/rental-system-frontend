<script setup lang="ts">
import { ref, reactive } from 'vue'
import axios from 'axios'
import { onBeforeMount } from 'vue'
// import VueAxios from 'vue-axios'
import { RouterLink, RouterView } from 'vue-router'
import ProductTable from '@/components/table/ProductTable.vue'
import PhotographerTable from './components/table/PhotographerTable.vue'
import OrderTable from './components/table/OrderTable.vue'
import SearchDateTable from './components/table/SearchDateTable.vue'

onBeforeMount(()=>{
  // getProductTable('female')
  // getProductTable('male')
  // getProductTable('acc')
})
const cate = {
  'female': ['FemaleProductId', 'FemaleProductName', 'FemaleProductInfo'],
  'male': ['MaleProductId', 'MaleProductName', 'MaleProductInfo'],
  'acc': ['AccProductId', 'AccProductName', 'AccProductInfo'],
  'photographer': ['PhotographerProductId', 'PhotographerProductName', 'PhotographerProductInfo'],
}

// 各数据表
// 商品
const femaleProductTable = ref(null)
const maleProductTable = ref(null)
const accProductTable = ref(null)
// 摄影师
const photographerTable = ref(null)
// 搜索结果
const searchProductTable = ref(null)
const searchDateTable = ref(null)
// 订单
const orderListTable = ref(null)
const orderDetailTable = ref(null)

// 各表单
// 商品表单
const productDialogVisible = ref(false)
const productDialogDisable = ref({
  "disableIdx": false,
  "disableName": false,
  "disableInfo": false,
})
const productForm = ref({
  "method": "",
  "cate": "",
  "idx": 0,
  "name": "",
  "count": 1,
  "info": "",
})
// 摄影师表单
const photographerDialogVisible = ref(false)
const photographerDialogDisable = ref({
  "disableIdx": false,
  "disableName": false,
  "disableInfo": false,
})
const photographerForm = ref({
  "method": "",
  "cate": "",
  "idx": 0,
  "name": "",
  "info": "",
})
// 订单表单
const orderDialogVisible = ref(false)
const orderDialogDisable = ref({
  "disableIdx": false,
  "disableName": false,
  "disableDate": false,
  "disableInfo": false,
})
const orderForm = ref({
  "method": "",
  "idx": 0,
  "name": "",
  "start_date": "",
  "end_date": "",
  "info": "",
})
// 订单明细表单
const orderDetailDialogVisible = ref(false)
const orderDetailDialogDisable = ref({
  "disableIdx": false,
  "disableOrderIdx": false,
  "disableCate": false,
  "disableProductIdx": false,
  "disableInfo": false,
})
const orderDetailForm = ref({
  "method": "",
  "idx": 0,
  "order_idx": 0,
  "cate": "",
  "product_idx": 0,
  "info": "",
})
// 按商品查询，返回可用日期。
const searchProductForm = ref({
  "method": "product",
  "cate": null,
  "idx": null,
  "start_date": null,
  "end_date": null,
})
// 按日期查询，返回可用商品。
const searchDateForm = ref({
  "method": "date",
  "cate": null,
  "start_date": null,
  "end_date": null,
})

// 按钮事件处理
const fillProductForm = (cate: string, index: number) => {
  if (cate=='male') {
    productForm.value.idx = maleProductTable.value[index].MaleProductId
    productForm.value.name = maleProductTable.value[index].MaleProductName
    productForm.value.info = maleProductTable.value[index].MaleProductInfo
  }
  else if (cate=='female') {
    productForm.value.idx = femaleProductTable.value[index].FemaleProductId
    productForm.value.name = femaleProductTable.value[index].FemaleProductName
    productForm.value.info = femaleProductTable.value[index].FemaleProductInfo
  }
  else if (cate=='acc') {
    productForm.value.idx = accProductTable.value[index].AccProductId
    productForm.value.name = accProductTable.value[index].AccProductName
    productForm.value.info = accProductTable.value[index].AccProductInfo
  }
}
// 处理商品
const handleProduct = (method: string, cate: string) => {
  productForm.value['method'] = method
  productForm.value['cate'] = cate
  axios.post(
    '/api/product',
    productForm.value
  ).then(response => {
      if(cate == 'female') {
        femaleProductTable.value = response.data
      }
      else if (cate == 'male') {
        maleProductTable.value = response.data
      }
      else if (cate == 'acc') {
        accProductTable.value = response.data
      }
    }
  )
  productForm.value['idx'] = 0
  productForm.value['name'] = ""
  productForm.value['info'] = ""
}
const handleProductDialog = (method: string, cate: string, index: number = 0) => {
  productDialogVisible.value = !productDialogVisible.value
  productForm.value.method = method
  productForm.value.cate = cate
  if (method=='insert') {
    productDialogDisable.value.disableIdx = !productDialogDisable.value.disableIdx
  }
  else if (method=='update') {
    productDialogDisable.value.disableIdx = !productDialogDisable.value.disableIdx
    fillProductForm(cate, index)
  }
  else if (method=='delete') {
    productDialogDisable.value.disableIdx = !productDialogDisable.value.disableIdx
    productDialogDisable.value.disableName = !productDialogDisable.value.disableName
    productDialogDisable.value.disableInfo = !productDialogDisable.value.disableInfo
    fillProductForm(cate, index)
  }
}
// 处理摄影师
const fillPhotographerForm = (cate: string = 'photographer', index: number) => {
    photographerForm.value.idx = photographerTable.value[index].PhotographerId
    photographerForm.value.name = photographerTable.value[index].PhotographerName
    photographerForm.value.info = photographerTable.value[index].PhotographerInfo
}
const handlePhotographer = (method: string, cate: string = 'photographer') => {
  photographerForm.value['method'] = method
  photographerForm.value['cate'] = cate
  axios.post(
    '/api/photographer',
    photographerForm.value
  ).then(response => {
    photographerTable.value = response.data
    }
  )
  photographerForm.value['idx'] = 0
  photographerForm.value['name'] = ""
  photographerForm.value['info'] = ""
}
const handlePhotographerDialog = (method: string, cate: string, index: number = 0) => {
  photographerDialogVisible.value = !photographerDialogVisible.value
  photographerForm.value.method = method
  photographerForm.value.cate = cate
  if (method=='insert') {
    photographerDialogDisable.value.disableIdx = !photographerDialogDisable.value.disableIdx
  }
  else if (method=='update') {
    photographerDialogDisable.value.disableIdx = !photographerDialogDisable.value.disableIdx
    fillPhotographerForm(cate, index)
  }
  else if (method=='delete') {
    photographerDialogDisable.value.disableIdx = !photographerDialogDisable.value.disableIdx
    photographerDialogDisable.value.disableName = !photographerDialogDisable.value.disableName
    photographerDialogDisable.value.disableInfo = !photographerDialogDisable.value.disableInfo
    fillPhotographerForm(cate, index)
  }
}
// 获取订单
const fillOrderForm = (index: number) => {
  orderForm.value.idx = orderListTable.value[index].OrderId
  orderForm.value.name = orderListTable.value[index].OrderName
  orderForm.value.start_date = orderListTable.value[index].StartDate
  orderForm.value.end_date = orderListTable.value[index].EndDate
  orderForm.value.info = orderListTable.value[index].OrderInfo
}
const handleOrder = (method: string, index: number = 0) => {
  orderForm.value['method'] = method
  axios.post(
    '/api/order/list', 
    orderForm.value
  ).then(response => {
    orderListTable.value = response.data
  })
  orderForm.value['idx'] = 0
  orderForm.value['name'] = ""
  orderForm.value['start_date'] = ""
  orderForm.value['end_date'] = ""
  orderForm.value['info'] = ""
}
const handleOrderDialog = (method: string, index: number = 0) => {
  orderDialogVisible.value = !orderDialogVisible.value
  orderForm.value.method = method
  if (method=='insert') {
    orderDialogDisable.value.disableIdx = !orderDialogDisable.value.disableIdx
  }
  else if (method=='update') {
    orderDialogDisable.value.disableIdx = !orderDialogDisable.value.disableIdx
    orderDialogDisable.value.disableDate = !orderDialogDisable.value.disableDate
    fillOrderForm(index)
  }
  else if (method=='delete') {
    orderDialogDisable.value.disableIdx = !orderDialogDisable.value.disableIdx
    orderDialogDisable.value.disableName = !orderDialogDisable.value.disableName
    orderDialogDisable.value.disableDate = !orderDialogDisable.value.disableDate
    orderDialogDisable.value.disableInfo = !orderDialogDisable.value.disableInfo
    fillOrderForm(index)
  }
}
// 获取订单明细
const fillOrderDetailForm = (index: number) => {
  console.log(orderDetailTable.value[index])
  orderDetailForm.value['idx'] = orderDetailTable.value[index].OrderDetailId
  orderDetailForm.value['order_idx'] = orderDetailTable.value[index].OrderId
  orderDetailForm.value['cate'] = orderDetailTable.value[index].Cate
  orderDetailForm.value['product_idx'] = orderDetailTable.value[index].ProductId
  orderDetailForm.value['info'] = orderDetailTable.value[index].OrderDetailInfo
}
const handleOrderDetail = (method: string, index: number = 0) => {
  orderDetailForm.value['method'] = method
  axios.post(
    '/api/order/detail', 
    orderDetailForm.value
  ).then(response => {
    orderDetailTable.value = response.data
    for(let orderDetail of orderDetailTable.value) {
      if(orderDetail.Cate == 'male') {
        orderDetail.CnCate = '男装'
      }
      else if(orderDetail.Cate == 'female') {
        orderDetail.CnCate = '女装'
      }
      else if(orderDetail.Cate == 'acc') {
        orderDetail.CnCate = '饰品'
      }
      else if(orderDetail.Cate == 'photographer') {
        orderDetail.CnCate = '摄影师'
      }
    }
    orderDetailForm.value['idx'] = 0
    orderDetailForm.value['order_idx'] = 0
    orderDetailForm.value['cate'] = ""
    orderDetailForm.value['product_idx'] = 0
    orderDetailForm.value['info'] = ""
  })
}
const handleOrderDetailDialog = (method: string, index: number = 0) => {
  orderDetailDialogVisible.value = !orderDetailDialogVisible.value
  orderDetailForm.value.method = method
  if (method=='insert') {
    orderDetailDialogDisable.value.disableIdx = !orderDetailDialogDisable.value.disableIdx
  }
  else if (method=='update') {
    orderDetailDialogDisable.value.disableIdx = !orderDetailDialogDisable.value.disableIdx
    orderDetailDialogDisable.value.disableOrderIdx = !orderDetailDialogDisable.value.disableOrderIdx
    orderDetailDialogDisable.value.disableCate = !orderDetailDialogDisable.value.disableCate
    orderDetailDialogDisable.value.disableProductIdx = !orderDetailDialogDisable.value.disableProductIdx
    fillOrderDetailForm(index)
  }
  else if (method=='delete') {
    orderDetailDialogDisable.value.disableIdx = !orderDetailDialogDisable.value.disableIdx
    orderDetailDialogDisable.value.disableOrderIdx = !orderDetailDialogDisable.value.disableOrderIdx
    orderDetailDialogDisable.value.disableCate = !orderDetailDialogDisable.value.disableCate
    orderDetailDialogDisable.value.disableProductIdx = !orderDetailDialogDisable.value.disableProductIdx
    orderDetailDialogDisable.value.disableInfo = !orderDetailDialogDisable.value.disableInfo
    fillOrderDetailForm(index)
  }
}
// 查询
const searchByProduct = () => {
  axios.post(
    '/api/search', 
    searchProductForm.value
  ).then(response => {
    // searchProductTable.value = response.value
    let res = []
    for (const [k, v] of Object.entries(response.data)) {
      if (v==false) {
        res.push(
          {
            "Date": k,
            "OrderId": "可用",
          }
        )
      }
      else {
        res.push(
          {
            "Date": k,
            "OrderId": v,
          }
        )
      }
    }
    searchProductTable.value = res
    console.log(searchProductTable.value)
  })
  // console.log(searchProductForm)
}
const searchByDate = () => {
  axios.post(
    '/api/search', 
    searchDateForm.value
  ).then(response => {
    searchDateTable.value = response.data
  })
}
</script>

<template>
  <h1>婚纱管理系统</h1>
  <div id="product">
    <div id="product_dialog">
      <el-dialog title="商品管理" v-model="productDialogVisible" :show-close="false" :close-on-click-modal="false" :close-on-press-escape="false" draggable>
        <el-form :model="productForm">
          <el-form-item label="执行操作" prop="method">
            <el-select v-model="productForm.method" :disabled="true">
              <el-option label="新增" value="insert" />
              <el-option label="修改" value="update" />
              <el-option label="删除" value="delete" />
            </el-select>
          </el-form-item>
          <el-form-item label="类别" prop="cate">
            <el-select v-model="productForm.cate" :disabled="true">
              <el-option label="女装" value="female" />
              <el-option label="男装" value="male" />
              <el-option label="装饰品" value="acc" />
            </el-select>
          </el-form-item>
          <el-form-item label="编号">
            <el-input v-model="productForm.idx" :disabled="productDialogDisable.disableIdx"/>
          </el-form-item>
          <el-form-item label="名称">
            <el-input v-model="productForm.name" :disabled="productDialogDisable.disableName"/>
          </el-form-item>
          <el-form-item label="相关信息">
            <el-input v-model="productForm.info" :disabled="productDialogDisable.disableInfo"/>
          </el-form-item>
          <el-form-item>
            <el-button type="success" @click="handleProduct(productForm.method, productForm.cate)">提交</el-button>
            <el-button type="danger" @click="handleProductDialog(productForm.method, productForm.cate)">取消</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>
    </div>
    <h2>商品管理</h2>
    <div id="female">
      <h3>女装</h3>
      <div id="female_table">
        <el-button plain type="primary" @click.prevent="handleProduct('query', 'female')">刷新商品</el-button>
        <el-button plain type="success" @click.prevent="handleProductDialog('insert', 'female')">新增商品</el-button>
        <el-table v-model:data="femaleProductTable" style="width: 100%" max-height="250" border>
          <el-table-column fixed prop="FemaleProductId" label="商品编号" width="200" sortable />
          <el-table-column fixed prop="FemaleProductName" label="商品名称" width="300" sortable />
          <el-table-column prop="FemaleProductInfo" label="相关信息" width="700" />
          <el-table-column fixed="right" label="操作" width="250">
              <template #default="scope">
                  <el-button plain type="warning" @click.prevent="handleProductDialog('update', 'female', scope.$index)">修改商品</el-button>
                  <el-button plain type="danger" @click.prevent="handleProductDialog('delete', 'female', scope.$index)">删除商品</el-button>
              </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <div id="male">
      <h3>男装</h3>
      <div id="male_table">
        <el-button plain type="primary" @click.prevent="handleProduct('query', 'male')">刷新商品</el-button>
        <el-button plain type="success" @click.prevent="handleProductDialog('insert', 'male')">新增商品</el-button>
        <el-table :data="maleProductTable" style="width: 100%" max-height="250" border>
          <el-table-column fixed prop="MaleProductId" label="商品编号" width="200" sortable />
          <el-table-column fixed prop="MaleProductName" label="商品名称" width="300" sortable />
          <el-table-column prop="MaleProductInfo" label="相关信息" width="700" />
          <el-table-column fixed="right" label="操作" width="250">
              <template #default="scope">
                  <el-button plain type="warning" @click.prevent="handleProductDialog('update', 'male', scope.$index)">修改商品</el-button>
                  <el-button plain type="danger" @click.prevent="handleProductDialog('delete', 'male', scope.$index)">删除商品</el-button>
              </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <div id="acc">
      <h3>装饰</h3>
      <div id="acc_table">
        <el-button plain type="primary" @click.prevent="handleProduct('query', 'acc')">刷新商品</el-button>
        <el-button plain type="success" @click.prevent="handleProductDialog('insert', 'acc')">新增商品</el-button>
        <el-table :data="accProductTable" style="width: 100%" max-height="250" border>
          <el-table-column fixed prop="AccProductId" label="商品编号" width="200" sortable />
          <el-table-column fixed prop="AccProductName" label="商品名称" width="300" sortable />
          <el-table-column prop="AccProductInfo" label="相关信息" width="700" />
          <el-table-column fixed="right" label="操作" width="250">
              <template #default="scope">
                  <el-button plain type="warning" @click.prevent="handleProductDialog('update', 'acc', scope.$index)">修改商品</el-button>
                  <el-button plain type="danger" @click.prevent="handleProductDialog('delete', 'acc', scope.$index)">删除商品</el-button>
              </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
  <div id="photographer">
    <div id="photographer_dialog">
      <el-dialog title="摄影师管理" v-model="photographerDialogVisible" :show-close="false" :close-on-click-modal="false" :close-on-press-escape="false" draggable>
        <el-form :model="photographerForm">
          <el-form-item label="执行操作" prop="method">
            <el-select v-model="photographerForm.method" :disabled="true">
              <el-option label="新增" value="insert" />
              <el-option label="修改" value="update" />
              <el-option label="删除" value="delete" />
            </el-select>
          </el-form-item>
          <el-form-item label="编号">
            <el-input v-model="photographerForm.idx" :disabled="photographerDialogDisable.disableIdx"/>
          </el-form-item>
          <el-form-item label="名称">
            <el-input v-model="photographerForm.name" :disabled="photographerDialogDisable.disableName"/>
          </el-form-item>
          <el-form-item label="相关信息">
            <el-input v-model="photographerForm.info" :disabled="photographerDialogDisable.disableInfo"/>
          </el-form-item>
          <el-form-item>
            <el-button type="success" @click="handlePhotographer(photographerForm.method, photographerForm.cate)">提交</el-button>
            <el-button type="danger" @click="handlePhotographerDialog(photographerForm.method, photographerForm.cate)">取消</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>
    </div>
    <h2>摄影师</h2>
    <div id="photographer_table">
        <el-button plain type="primary" @click.prevent="handlePhotographer('query', 'photographer')">刷新摄影师</el-button>
        <el-button plain type="success" @click.prevent="handlePhotographerDialog('insert', 'photographer')">新增摄影师</el-button>
        <el-table :data="photographerTable" style="width: 100%" max-height="250" border>
          <el-table-column fixed prop="PhotographerId" label="摄影师编号" width="200" sortable />
          <el-table-column fixed prop="PhotographerName" label="摄影师名称" width="300" sortable />
          <el-table-column prop="PhotographerInfo" label="相关信息" width="700" />
          <el-table-column fixed="right" label="操作" width="250">
              <template #default="scope">
                  <el-button plain type="warning" @click.prevent="handlePhotographerDialog('update', 'photographer', scope.$index)">修改摄影师</el-button>
                  <el-button plain type="danger" @click.prevent="handlePhotographerDialog('delete', 'photographer', scope.$index)">删除摄影师</el-button>
              </template>
          </el-table-column>
        </el-table>
      </div>
  </div>
  <div id="search">
    <h2>搜索</h2>
    <div id="search_by_product">
      <h3>按商品搜索</h3>
      <div id="search_product_form">
        <el-form :model="searchProductForm" label-width="auto" style="max-width: 600px">
          <el-form-item label="开始日期">
            <el-date-picker v-model="searchProductForm.start_date" type="date" placeholder="开始日期" value-format="YYYY-MM-DD" style="max-width: 600px" />
          </el-form-item>
          <el-form-item label="结束日期">
            <el-date-picker v-model="searchProductForm.end_date" type="date" placeholder="结束日期" value-format="YYYY-MM-DD" style="max-width: 600px" />
          </el-form-item>
          <el-form-item label="选择类别" style="max-width: 600px">
            <el-select v-model="searchProductForm.cate" placeholder="选择类别" >
              <el-option label="女装" value="female" />
              <el-option label="男装" value="male" />
              <el-option label="装饰品" value="acc" />
              <el-option label="摄影师" value="photographer" />
            </el-select>
          </el-form-item>
          <el-form-item label="编号">
            <el-input v-model.number="searchProductForm.idx" />
          </el-form-item>
        </el-form>
      </div>
      <div id="search_product_table">
        <el-button plain type="primary" @click.prevent="searchByProduct()">查询</el-button>
        <el-table :data="searchProductTable" style="width: 100%" max-height="500" border>
          <el-table-column fixed prop="Date" label="日期" width="700" sortable />
          <el-table-column prop="OrderId" label="占用订单" width="700" sortable />
        </el-table>
      </div>
    </div>
    <div id="search_by_date">
      <h3>按日期搜索</h3>
      <div id="search_date_form">
        <el-form :model="searchDateForm" label-width="auto" style="max-width: 600px">
          <el-form-item label="开始日期">
            <el-date-picker v-model="searchDateForm.start_date" type="date" placeholder="开始日期" value-format="YYYY-MM-DD" style="max-width: 600px" />
          </el-form-item>
          <el-form-item label="结束日期">
            <el-date-picker v-model="searchDateForm.end_date" type="date" placeholder="结束日期" value-format="YYYY-MM-DD" style="max-width: 600px" />
          </el-form-item>
          <el-form-item label="选择类别" style="max-width: 600px">
            <el-select v-model="searchDateForm.cate" placeholder="选择类别" >
              <el-option label="女装" value="female" />
              <el-option label="男装" value="male" />
              <el-option label="装饰品" value="acc" />
              <el-option label="摄影师" value="photographer" />
            </el-select>
          </el-form-item>
        </el-form>
        <div id="search_date_table">
          <el-button plain type="primary" @click.prevent="searchByDate()">查询</el-button>
          <el-table :data="searchDateTable" style="width: 100%" max-height="500" border>
            <el-table-column fixed prop="MaleProductId" label="编号" width="500" sortable />
            <el-table-column prop="MaleProductName" label="名称" width="500" sortable />
            <el-table-column prop="MaleProductInfo" label="相关信息" width="500" />
          </el-table>
        </div>
      </div>
    </div>
  </div>
  <div id="order">
    <div id="order_dialog">
      <el-dialog title="订单管理" v-model="orderDialogVisible" :show-close="false" :close-on-click-modal="false" :close-on-press-escape="false" draggable>
        <el-form :model="orderForm">
          <el-form-item label="执行操作" prop="method">
            <el-select v-model="orderForm.method" :disabled="true">
              <el-option label="新增" value="insert" />
              <el-option label="修改" value="update" />
              <el-option label="删除" value="delete" />
            </el-select>
          </el-form-item>
          <el-form-item label="编号">
            <el-input v-model="orderForm.idx" :disabled="orderDialogDisable.disableIdx"/>
          </el-form-item>
          <el-form-item label="名称">
            <el-input v-model="orderForm.name" :disabled="orderDialogDisable.disableName"/>
          </el-form-item>
          <el-form-item label="开始日期">
            <el-date-picker v-model="orderForm.start_date" type="date" placeholder="开始日期" value-format="YYYY-MM-DD" style="max-width: 600px" :disabled="orderDialogDisable.disableDate"/>
          </el-form-item>
          <el-form-item label="结束日期">
            <el-date-picker v-model="orderForm.end_date" type="date" placeholder="结束日期" value-format="YYYY-MM-DD" style="max-width: 600px" :disabled="orderDialogDisable.disableDate"/>
          </el-form-item>
          <el-form-item label="相关信息">
            <el-input v-model="orderForm.info" :disabled="orderDialogDisable.disableInfo"/>
          </el-form-item>
          <el-form-item>
            <el-button type="success" @click="handleOrder(orderForm.method)">提交</el-button>
            <el-button type="danger" @click="handleOrderDialog(orderForm.method)">取消</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>
    </div>
    <h2>订单</h2>
    <div id="order_list">
      <h3>当前订单</h3>
      <div id="order_list_table">
        <el-button plain type="primary" @click.prevent="handleOrder('query')">刷新订单</el-button>
        <el-button plain type="success" @click.prevent="handleOrderDialog('insert')">新增订单</el-button>
        <el-table :data="orderListTable" style="width: 100%" max-height="250" border>
          <el-table-column fixed prop="OrderId" label="订单编号" width="200" sortable />
          <el-table-column fixed prop="OrderName" label="订单名称" width="300" sortable />
          <el-table-column prop="StartDate" label="开始日期" width="300" sortable />
          <el-table-column prop="EndDate" label="结束日期" width="300" sortable />
          <el-table-column prop="OrderInfo" label="相关信息" width="700" />
          <el-table-column fixed="right" label="操作" width="250">
            <template #default="scope">
                <el-button plain type="warning" @click.prevent="handleOrderDialog('update', scope.$index)">修改订单</el-button>
                <el-button plain type="danger" @click.prevent="handleOrderDialog('delete', scope.$index)">删除订单</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <div id="order_detail">
      <div id="order_detail_dialog">
      <el-dialog title="订单明细管理" v-model="orderDetailDialogVisible" :show-close="false" :close-on-click-modal="false" :close-on-press-escape="false" draggable>
        <el-form :model="orderDetailForm">
          <el-form-item label="执行操作" prop="method">
            <el-select v-model="orderDetailForm.method" :disabled="true">
              <el-option label="新增" value="insert" />
              <el-option label="修改" value="update" />
              <el-option label="删除" value="delete" />
            </el-select>
          </el-form-item>
          <el-form-item label="订单明细编号">
            <el-input v-model="orderDetailForm.idx" :disabled="orderDetailDialogDisable.disableIdx"/>
          </el-form-item>
          <el-form-item label="所属订单编号">
            <el-input v-model="orderDetailForm.order_idx" :disabled="orderDetailDialogDisable.disableOrderIdx"/>
          </el-form-item>
          <el-form-item label="类别" prop="cate">
            <el-select v-model="orderDetailForm.cate" :disabled="orderDetailDialogDisable.disableCate">
              <el-option label="女装" value="female" />
              <el-option label="男装" value="male" />
              <el-option label="装饰品" value="acc" />
              <el-option label="摄影师" value="photographer" />
            </el-select>
          </el-form-item>
          <el-form-item label="编号">
            <el-input v-model="orderDetailForm.product_idx" :disabled="orderDetailDialogDisable.disableProductIdx"/>
          </el-form-item>
          <el-form-item label="相关信息">
            <el-input v-model="orderDetailForm.info" :disabled="orderDetailDialogDisable.disableInfo"/>
          </el-form-item>
          <el-form-item>
            <el-button type="success" @click="handleOrderDetail(orderDetailForm.method)">提交</el-button>
            <el-button type="danger" @click="handleOrderDetailDialog(orderDetailForm.method)">取消</el-button>
          </el-form-item>
        </el-form>
      </el-dialog>
    </div>
      <h3>订单明细</h3>
      <div id="order_detail_table">
        <el-button plain type="primary" @click.prevent="handleOrderDetail('query')">刷新订单明细</el-button>
        <el-button plain type="success" @click.prevent="handleOrderDetailDialog('insert')">新增订单明细</el-button>
        <el-table :data="orderDetailTable" style="width: 100%" max-height="250" border>
          <el-table-column fixed prop="OrderDetailId" label="订单明细编号" width="200" sortable />
          <el-table-column prop="OrderId" label="对应订单编号" width="300" sortable />
          <el-table-column prop="CnCate" label="类别" width="300" sortable />
          <el-table-column prop="ProductId" label="编号" width="300" sortable />
          <el-table-column prop="OrderDetailInfo" label="相关信息" width="700" />
          <el-table-column fixed="right" label="操作" width="300">
              <template #default="scope">
                  <el-button plain type="warning" @click.prevent="handleOrderDetailDialog('update', scope.$index)">修改订单明细</el-button>
                  <el-button plain type="danger" @click.prevent="handleOrderDetailDialog('delete', scope.$index)">删除订单明细</el-button>
              </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
