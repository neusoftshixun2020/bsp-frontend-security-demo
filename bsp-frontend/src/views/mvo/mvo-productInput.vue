<template>
  <div style="margin-left:30px; margin-right: 30px">
    <br>
    <br>

    <el-row :gutter="6">
      <el-col :span="7">
        <div class="grid-content">
          <span style="color:#606266">
            Product Title
            <el-input v-model="scondition" autocomplete="off" style="width:280px" placeholder="please enter product title" />
          </span>
        </div>
      </el-col>
      <el-col :span="1">
        <div class="grid-content">
          <el-button type="success" icon="el-icon-search" circle @click="searchProduct" />
        </div>
      </el-col>
      <el-col :span="14"><div class="grid-content" /></el-col>
      <el-col :span="1">
        <div class="grid-content">
          <div class="grid-content"><el-button type="primary" plain icon="el-icon-plus" @click="addProduct">add</el-button></div>
        </div>
      </el-col>
    </el-row>

    <el-table :data="filteredProductData" style="width: 100%">
      <el-table-column prop="pro_id" sortable label="Product ID" />
      <el-table-column prop="sku_cd" label="SKU Code" />
      <el-table-column prop="title" label="Title" />
      <el-table-column prop="model" label="Model" />
      <el-table-column prop="price.price" label="Price" />

      <el-table-column label="Operation">
        <template slot-scope="scope">
          <el-button type="primary" icon="el-icon-edit" circle @click="modifyProduct(scope.row)" />
          <el-button type="danger" icon="el-icon-delete" circle @click="deleteProduct(scope.row)" />
        </template>
      </el-table-column>
    </el-table>

    <el-row :gutter="20">
      <el-col :span="8"><div class="grid-content" /></el-col>
      <el-col :span="8">
        <div class="grid-content">
          <div class="block">
            <el-pagination
              :page-size="20"
              layout="total, prev, pager, next"
              :total="1000"
            />
          </div>
        </div></el-col>
      <el-col :span="8"><div class="grid-content" /></el-col>
    </el-row>

    <el-dialog :visible.sync="dialogVisible" width="70%" :close-on-lick-modal="false">
      <div>
        <el-form ref="addFormData" :model="addFormData"  label-width="100px" class="">
          <el-row>
            <el-col>
              <el-form-item label="SKU Code" label-width="160px">
                <el-input v-model="addFormData.product.sku_cd" type="text" autocomplete="off" clearable />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="Brand ID" label-width="160px">
                <el-input v-model="addFormData.product.brd_id" type="text" autocomplete="off" clearable />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="Manufacturer ID" label-width="160px">
                <el-input v-model="addFormData.product.man_id" type="text" autocomplete="off" clearable />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="Title" label-width="160px">
                <el-input v-model="addFormData.product.title" type="text" autocomplete="off" clearable />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="UPC Code" label-width="160px">
                <el-input v-model="addFormData.product.upc" type="text" autocomplete="off" clearable />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="ENA Code" label-width="160px">
                <el-input v-model="addFormData.product.ean" type="text" autocomplete="off" clearable />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="Model" label-width="160px">
                <el-input v-model="addFormData.product.model" type="text" autocomplete="off" clearable />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="2">
            <el-col :span="6">
              <div class="grid-content">
                <el-form-item label="Length(cm)" label-width="160px">
                  <el-input v-model="addFormData.product.packageInfo.length" style="width:150px" type="text" autocomplete="off" clearable />
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content">
                <el-form-item label="Width(cm)" label-width="140px">
                  <el-input v-model="addFormData.product.packageInfo.width" style="width:150px" type="text" autocomplete="off" clearable />
                </el-form-item>
              </div>
            </el-col>

            <el-col :span="6">
              <div class="grid-content">
                <el-form-item label="Height(cm)" label-width="130px">
                  <el-input v-model="addFormData.product.packageInfo.height" style="width:150px" type="text" autocomplete="off" clearable />
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content">
                <el-form-item label="Weight(kg)" label-width="130px">
                  <el-input v-model="addFormData.product.packageInfo.weight" style="width:150px" type="text" autocomplete="off" clearable />
                </el-form-item>
              </div>
            </el-col>
          </el-row>
          <el-row :gutter="40">
            <el-col :span="6">
              <div class="grid-content">
                <el-form-item label="Retail Price" label-width="160px">
                  <el-input v-model="addFormData.product.price.price" style="width: 150px" type="text" autocomplete="off" clearable />
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="9">
              <div class="grid-content">
                <el-form-item label="Effective Start Date" label-width="195px">
                  <el-date-picker
                    v-model="addFormData.product.price.effective_start_date"
                    type="date"
                    placeholder="选择日期"
                  />
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="9">
              <div class="grid-content">
                <el-form-item label="Effective End Date" label-width="180px">
                  <el-date-picker
                    v-model="addFormData.product.price.effective_end_date"
                    type="date"
                    placeholder="选择日期"
                  />
                </el-form-item>
              </div>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="Replenishment Peroid" label-width="160px">
                <el-input v-model="addFormData.product.replenishment_period" type="text" autocomplete="off" clearable />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <el-col :span="6">
              <div class="grid-content">
                <el-form-item label="Warranty day" label-width="160px">
                  <el-input v-model="addFormData.product.warranty_day" style="width: 150px" type="text" autocomplete="off" clearable />
                </el-form-item>
              </div>
            </el-col>
            <el-col :span="6">
              <div class="grid-content">
                <el-form-item label="Time Unit" label-width="140px">
                  <el-input v-model="addFormData.product.time_unit" style="width: 150px" type="text" autocomplete="off" clearable />
                </el-form-item>
              </div>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="Warranty Policy" label-width="160px">
                <el-input
                  v-model="addFormData.product.warranty"
                  type="textarea"
                  :autosize="{ minRows: 5, maxRows: 10}"
                  maxlength="1200"
                  show-word-limit
                  autocomplete="off"
                  clearable
                />
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col>
              <el-form-item label="Description" label-width="160px">
                <el-input
                  v-model="addFormData.product.productDescription.description"
                  type="textarea"
                  autocomplete="off"
                  :autosize="{ minRows: 5, maxRows: 10}"
                  maxlength="1200"
                  show-word-limit
                  clearable
                />
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
        <el-row :gutter="20">
          <el-col :span="20">
            <div class="grid-content" />
          </el-col>
          <el-col :span="2">
            <div class="grid-content">
              <el-button
                type="info"
                @click.native="closeDialog"
              >Close</el-button>
            </div>
          </el-col>
          <el-col :span="2">
            <div class="grid-content">
              <el-button type="primary" @click.native="addOrUpdateProduct">Submit</el-button>
            </div>
          </el-col>
        </el-row>
      </div>

    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'ProductInput',
  data() {
    return {
      productList: [],
      dialogVisible: false,
      scondition: '',
      addFormData: {
        product: {
          pro_id: '',
          sku_cd: '',
          brd_id: '',
          man_id: '',
          title: '',
          upc: '',
          ean: '',
          model: '',
          warranty_day: '',
          warranty: '',
          replenishment_period: '',
          time_unit: '',
          img_id: '',
          prc_id: '',
          productDescription: {
            description: ''
          },
          packageInfo: {
            length: '',
            width: '',
            height: '',
            weight: ''
          },
          price: {
            price: '',
            effective_start_date: '',
            effective_end_date: ''
          }

        },
        operationFlag: 'add'
      }
    }
  },
  mounted: function() {
    this.loadData()
  },
  methods: {
    loadData() {
      this.$store.dispatch('GetProducts').then((result) => {
        console.log(result)
        this.productList = result.data.list
      })
    },
    addProduct(rowData) {
     this.addFormData.operationFlag = "add"
      this.dialogVisible = true
    },
    searchProduct(rowData) {
      this.$store.dispatch('GetProductByTitle', this.scondition).then((result) => {
        this.productList = result.data
      })
    },
    modifyProduct(rowData) {
      this.dialogVisible = true
      this.addFormData.product.pro_id = rowData.pro_id
      this.addFormData.product.sku_cd = rowData.sku_cd
      this.addFormData.product.brd_id = rowData.brd_id
      this.addFormData.product.man_id = rowData.man_id
      this.addFormData.product.title = rowData.title
      this.addFormData.product.upc = rowData.upc
      this.addFormData.product.ean = rowData.ean
      this.addFormData.product.model = rowData.model
      this.addFormData.product.warranty_day = rowData.warranty_day
      this.addFormData.product.warranty = rowData.warranty
      this.addFormData.product.replenishment_period = rowData.replenishment_period
      this.addFormData.product.time_unit = rowData.time_unit
      this.addFormData.product.img_id = rowData.img_id
      this.addFormData.product.prc_id = rowData.prc_id
      this.addFormData.product.productDescription.description = rowData.productDescription.description
      this.addFormData.product.productDescription.pdn_id = rowData.productDescription.pdn_id
      this.addFormData.product.productDescription.pro_id = rowData.productDescription.pro_id
      this.addFormData.product.packageInfo.weight = rowData.packageInfo.weight
      this.addFormData.product.packageInfo.height = rowData.packageInfo.height
      this.addFormData.product.packageInfo.width = rowData.packageInfo.width
      this.addFormData.product.packageInfo.length = rowData.packageInfo.length
      this.addFormData.product.packageInfo.pck_id = rowData.packageInfo.pck_id
      this.addFormData.product.packageInfo.pro_id = rowData.packageInfo.pro_id
      this.addFormData.product.price.price = rowData.price.price
      this.addFormData.product.price.effective_start_date = rowData.price.effective_start_date
      this.addFormData.product.price.effective_end_date = rowData.price.effective_end_date
      this.addFormData.product.price.ofp_id = rowData.price.ofp_id
      this.addFormData.product.price.pro_id = rowData.price.pro_id
      this.addFormData.operationFlag = 'modify'
    },
    addOrUpdateProduct() {
      this.$refs.addFormData.validate(valid => {
        if (valid) {
          this.addFormData.product.pro_id = parseInt(this.addFormData.product.pro_id)
          this.addFormData.product.brd_id = parseInt(this.addFormData.product.brd_id)
          this.addFormData.product.man_id = parseInt(this.addFormData.product.man_id)
          this.addFormData.product.img_id = parseInt(this.addFormData.product.img_id)
          this.addFormData.product.prc_id = parseInt(this.addFormData.product.prc_id)

          console.log(this.addFormData)

          this.$store.dispatch('AddAndUpdateProduct', this.addFormData).then((result) => {
            if (result.code == 200) {
              this.$message({
                type: 'info',
                message: `Succeeded`
              })
            } else {
              this.$message({
                type: 'info',
                message: `Failed`
              })
            }
            this.dialogVisible = false
            this.loadData()
          })
        }
      })
    },
    updateProduct(rowData) {
      this.$refs.addFormData.validate(valid => {
        if (valid) {
          this.$store.dispatch('AddAndUpdateProduct', this.addFormData).then((result) => {
            if (result.data.data) {
              this.$message({
                type: 'info',
                message: `Succeeded`
              })
            } else {
              this.$message({
                type: 'info',
                message: `Failed`
              })
            }
            this.dialogVisible = false
            this.loadData()
          })
        }
      })
    },
    closeDialog() {
      this.dialogVisible = false
    },
    deleteProduct(rowData) {
      this.$confirm('Do you want to delete this record', 'Record delete', {
        confirmButtonText: 'Confirm',
        cancelButtonText: 'Cancel',
        type: 'warning'
      }).then(() => {
        this.$store.dispatch('DeleteProduct', rowData).then((result) => {
          if (result.code === 200) {
            this.$message({
              type: 'info',
              message: `Record deleted`
            })
          } else {
            this.$message({
              type: 'info',
              message: `Delete failed`
            })
          }
          this.loadData()
        })
      }).catch(() => {
      })
    }
  },
  computed: {
    filteredProductData () {
      const search = this.scondition
      return this.productList.filter(data => {
        return Object.keys(data).some(key => {
          return (
            String(data[key])
              .toLowerCase()
              .indexOf(search) > -1
          )
        })
      })
      // return this.tableDataDisease
    }
  },
}

</script>

<style>
  .container{
    margin-left:20px;

  }
  .top {
    display: flex;
    flex-direction: row;
    width: 20%;
    font-size: 15px;
    font-weight: bold;
    justify-content: space-between;
  }

  .el-row {
    margin-bottom: 20px;
  &:last-child {
     margin-bottom: 0;
   }
  }
  .el-col {
    border-radius: 4px;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
  }
</style>
