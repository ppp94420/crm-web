<template>
  <Dialog :model-value="dialogVisible" :title="dialogProps.title" :max-height="dialogProps.maxHeight" :cancel-dialog="cancelDialog" width="70%" top="8vh">
    <ProductManage :is-show-header="false" ref="productManageRef" />
    <template #footer>
      <el-button @click="cancelDialog">取消</el-button>
      <el-button type="primary" @click="getProductData()">确定</el-button>
    </template>
  </Dialog>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Dialog } from '@/components/Dialog'
import ProductManage from '@/views/Product/ProductManage.vue'
import { ElMessage } from 'element-plus'

interface DialogProps {
  title: string
  row: any
  maxHeight?: number | string
}

const productManageRef = ref()
const dialogVisible = ref(false)
const dialogProps = ref<DialogProps>({
  title: '选择商品',
  row: {},
  maxHeight: '500px'
})

const acceptParams = (params: DialogProps): void => {
  dialogProps.value = { ...dialogProps.value, ...params }
  dialogVisible.value = true
}

defineExpose({
  acceptParams
})

const cancelDialog = () => {
  dialogVisible.value = false
}

const emit = defineEmits(['getProductData'])

const getProductData = () => {
  if (productManageRef.value.proTable.selectedListIds.length > 1) {
    ElMessage.warning({ message: '只能选择一个商品' })
  } else if (productManageRef.value.proTable.selectedListIds.length === 1) {
    const selectedProduct = productManageRef.value.proTable.selectedList[0]
    const param = {
      pId: selectedProduct.id,
      pName: selectedProduct.name,
      price: selectedProduct.price,
      count: 1,
      totalPrice: selectedProduct.price
    }
    emit('getProductData', param)
    dialogVisible.value = false
  }
}
</script>
