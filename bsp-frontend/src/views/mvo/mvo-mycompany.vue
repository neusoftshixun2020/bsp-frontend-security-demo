<template>
  <div class="app-container">
<!--公司信息-->
    <el-col :span = '20' class = 'toolbar'>
      <p v-text ='head'></p>
    </el-col>

    <div class="ProductTable">
      <el-table
        ref="multipleTable"
        :data="companylist"
        element-loading-text="Loading"
        fit
        border
        highlight-current-row
        width="80%"
      >
      <el-table-column align="center" prop = 'man_id' label = 'Company ID'>
        </el-table-column>
        <el-table-column align="center" prop = 'name_cn' label = 'Company Name(CN)'>
        </el-table-column>
        <el-table-column align="center" prop = 'name_en' label = 'Company Name(EN)'>
        </el-table-column>
        <el-table-column align="center" prop = 'gmc_report_type' label = 'GMC Report Type'>
        </el-table-column>
        <el-table-column align="center" prop = 'gmc_report_url' label = 'GMC Report Url'>
        </el-table-column>
         <el-table-column align="center"  label = 'Operations'>
         <template slot-scope = 'scope'>
           <el-button type = 'primary' size="small" @click = 'EditCompany(scope.row)'>Modify</el-button>
         </template>
       </el-table-column>
      </el-table>
      <br>
      <el-button type="primary" @click="showAddInfo" style="margin-left:10px">Add</el-button>
    </div>

<!--品牌信息-->
    <div style="margin:20px;margin-left:10px">
      Brand Information
    </div>
    <div class="BrandTable">
      <el-table
        ref="multipleTable1"
        :data="brandList"
        element-loading-text="Loading"
        fit
        border
        highlight-current-row
        width="80%"
      >
        <el-table-column type="selection" />
        <el-table-column align="center" prop = 'man_id' label = 'Company ID'>
        </el-table-column>
         <el-table-column align="center" prop = 'name_en' label = 'Brand Name(EN)'>
        </el-table-column>
          <el-table-column align="center" label="image"  >
          <template slot-scope="scope">
            <img :src="scope.row.img_url" width="100" height="100" />
          </template>
        </el-table-column>
           <el-table-column align="center"  label = 'Operations'>
          <template slot-scope="scope">
            <el-button type = 'info' size="mini" @click.native ='EditBrand(scope.row)'>edit</el-button>
            <el-button type = 'danger' size="mini" @click.native ='deleteBrand(scope.row)'>delete</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
        <br>
      <el-button type="primary" @click="showaddBrand" style="margin-left:90px">Add</el-button>


    <!--修改company弹窗-->
    <el-dialog title='Edit Company Information' :visible.sync = 'dialogVisible' width = '50%' :close-on-lick-modal = 'false'>
      <el-form :model = 'ProductData'  ref = 'ProductData' label-width = '0px' class = ''>

        <el-form-item label="Company Name(CN)" label-width="130px"  prop='name_cn'>
          <el-col :span="8">
            <el-input type='text' v-model='ProductData.name_cn'  autocomplete='off' placeholder='Title'>
            </el-input>
          </el-col>
        </el-form-item>

        <el-form-item label="Company Name(EN)" label-width="130px"  prop='name_en'>
          <el-col :span="8">
            <el-input type='text' v-model='ProductData.name_en'  autocomplete='off' placeholder='Title'>
            </el-input>
          </el-col>
        </el-form-item>

         <el-form-item label="Brief Introdution"  label-width="130px" prop='decription'>
      <el-input
        v-model="ProductData.decription"
        :autosize="{ minRows: 8, maxRows: 8}"
        type="textarea"
        placeholder="enter"
      />
    </el-form-item>

        <el-form-item label="GMC Report Type(1-TUV , 2-UL)" label-width="130px"  prop='gmc_report_type'>
         <el-col :span="8">
            <el-input type='text' v-model='ProductData.gmc_report_type'  autocomplete='off' placeholder='Type'>
            </el-input>
          </el-col>
        </el-form-item>
         <el-form-item label="GMC Report Url" label-width="130px" prop='gmc_report_url'>
         <el-col :span="8">
            <el-input type='text' v-model='ProductData.gmc_report_url'  autocomplete='off' placeholder='Url'>
            </el-input>
          </el-col>
        </el-form-item>
      </el-form>
      <span slot = 'footer' class = 'dialog-footer'>
          <el-button type = 'primary' size='small' @click="updateCompany">Save</el-button>
          <el-button type = 'danger' size='small' @click.native = "dialogVisible = false, ProductData = {
                     name_cn:'',
                    name_en:'',
                    description:'',
                    gmc_report_type :'',
                    gmc_report_url:'' }">Close</el-button>
       </span>
    </el-dialog>

 <!--添加brand弹窗-->
    <el-dialog title='Add Brand' :visible.sync = 'dialogVisible1' width = '50%' :close-on-lick-modal = 'false'>
      <el-form :model = 'BrandData'  ref = 'BrandData' label-width = '0px' class = ''>
         <el-form-item label="Company ID" label-width="130px"  prop='man_id'>
          <el-col :span="8">
            <el-input type='text' v-model='BrandData.man_id'  autocomplete='off' placeholder='Title'>
            </el-input>
          </el-col>
        </el-form-item>
        <el-form-item label="Brand Name(EN)" label-width="130px"  prop='name_en'>
          <el-col :span="8">
            <el-input type='text' v-model='BrandData.name_en'  autocomplete='off' placeholder='Title'>
            </el-input>
          </el-col>
        </el-form-item>

         <el-form-item label-width="130px" >
         <div class="divcss5">Recommended image size 160*80 JPG/PNG format</div>
        </el-form-item>

        <el-form-item label="Brand Logo" label-width="130px" prop='image_url'>
          <el-upload
            action="http://localhost:8088/image/uploadImage"
            list-type="picture-card"
            :on-preview="handlePictureCardPreview"
            :on-success="handleUploadSuccess"
            :on-remove="handleRemove">
            <i class="el-icon-plus"></i>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt="">
          </el-dialog>
        </el-form-item>

      </el-form>
      <span slot = 'footer' class = 'dialog-footer'>
        <!-- <el-button size='small' @click="resetForm('BrandData')">Reset</el-button> -->
          <el-button type = 'primary' size='small' @click="addBrand">Save</el-button>
          <el-button type = 'danger' size='small' @click.native = "dialogVisible1 = false, BrandData = {
                    name_en:'',
                    image_url:'' }">Close</el-button>
       </span>
    </el-dialog>


<!--修改brand弹窗-->
    <el-dialog title='Edit Brand' :visible.sync = 'dialogVisible2' width = '50%' :close-on-lick-modal = 'false'>
      <el-form :model = 'BrandData'  ref = 'BrandData' label-width = '0px' class = ''>
        <el-form-item label="Company ID" label-width="130px"  prop='man_id'>
          <el-col :span="8">
            <el-input type='text' v-model='BrandData.man_id'  autocomplete='off' placeholder='Title'>
            </el-input>
          </el-col>
        </el-form-item>
        <el-form-item label="Brand Name(EN)" label-width="130px"  prop='name_en'>
          <el-col :span="8">
            <el-input type='text' v-model='BrandData.name_en'  autocomplete='off' placeholder='Title'>
            </el-input>
          </el-col>
        </el-form-item>

         <el-form-item label-width="130px" >
         <div class="divcss5">Recommended image size 160*80 JPG/PNG format</div>
        </el-form-item>

        <el-form-item label="Brand Logo" label-width="130px" prop='image_url'>
          <el-upload
            action="http://localhost:8088/image/uploadImage"
            list-type="picture-card"
            :on-preview="handlePictureCardPreview"
            :on-success="handleUploadSuccess"
            :on-remove="handleRemove">
            <i class="el-icon-plus"></i>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt="">
          </el-dialog>
        </el-form-item>

      </el-form>
      <span slot = 'footer' class = 'dialog-footer'>
         <!-- <el-button size='small' @click="resetForm('BrandData')">Reset</el-button> -->
          <el-button type = 'primary' size='small' @click="updateBrand">Save</el-button>
          <el-button type = 'danger' size='small' @click.native = "dialogVisible2 = false, BrandData = {
                    name_en:'',
                    image_url:'' }">Close</el-button>
       </span>
    </el-dialog>
  </div>

</template>



<script>
export default {
  name: 'MergeHeader',
  data() {
    return {
      companylist: [],
      brandList:[],
      downloadLoading: false,
      dialogVisible:false,
      dialogVisible1:false,
      dialogVisible2:false,
      head:'Company Information',
      dialogImageUrl: '',
      img_id: '',

      ProductData:{
          man_id:'',
          name_cn:'',
          name_en:'',
          decription:'',
          gmc_report_type:'',
          gmc_report_url:'',
      },
       BrandData:{
          man_id:'',
          brd_id:'',
          name_en:'',
          image_url:''

      }
     }
  },
   mounted: function () {
    this.loadData()
  },
  methods: {
  //   uploadImage:{

  //   },
  // chooseImage:{

  // },
    loadData () {
      this.$store.dispatch('GetAllByFilter',this.ProductData.man_id).then((result) => {
        // console.log("result.data-----companylist")
        // console.log(result.data)
        // console.log("result.data.list-----companylist")
        this.companylist = result.data.list
        // console.log(result.data.list)
      })
      this.$store.dispatch('GetBrandByFilter',this.ProductData.man_id).then((result) => {
        // console.log("result.data-----brandList")
        // console.log(result.data)
        // console.log("result.data.list-----brandList")
        // console.log(result.data.list)
        this.brandList = result.data
        console.log("branddata", this.brandList)
      })
    },

    EditCompany(rowData){
      this.ProductData = Object.assign({}, rowData)
      this.dialogVisible = true

    },
    updateCompany(){
      this.$refs.ProductData.validate(valid => {
        if(valid) {
          // console.log('the parameter is valid');
          this.$store.dispatch('UpdateManufacturer',this.ProductData).then((result) => {
            // console.log("result.code:"+result.code)
            if (result.code==200){
              this.$message({
                type: 'info',
                message: `update operation succeeded`
              })
            }else{
              this.$message({
                type: 'info',
                message: `update operation failed`
              })
            }
            this.dialogVisible = false
            this.loadData()
          })
        } else {
          // console.log('the parameter is invalid')
          return false
        }
      })
    },
    showaddBrand(){
      this.dialogVisible1 = true
    },
    showAddInfo() {
      this.$router.push({path: "mvo-myInfo"})
    },
    addBrand(){
    this.$refs.BrandData.validate(valid => {
        if(valid) {
          // console.log('valid');
          this.BrandData.img_url = this.dialogImageUrl
          this.$store.dispatch('AddBrand',this.BrandData).then((result) => {
            if (result.code==200){
              this.$message({
                type: 'info',
                message: `add operation succeeded`
              })
              this.loadData()
              this.dialogVisible1 = false
              this.$refs.BrandData.resetFields()
            }else{
              this.$message({
                type: 'info',
                message: `add operation failed`
              })
            }

          })
        } else {
          // console.log('the parameter is invalid');
          return false
        }
      })
    },
    EditBrand(rowData){
      this.BrandData = Object.assign({}, rowData)
      this.dialogVisible2 = true
    },
    updateBrand(){
    this.$refs.BrandData.validate(valid => {
        if(valid) {
          this.BrandData.img_url = this.dialogImageUrl
          // console.log("BrandData", this.BrandData)
          // console.log("this.$ref.brandData", this.$refs.BrandData)
          this.$store.dispatch('UpdateBrand',this.BrandData).then((result) => {
            // console.log(result)
            if (result.code==200){
              this.$message({
                type: 'info',
                message: `update operation succeeded`
              })
            }else{
              this.$message({
                type: 'info',
                message: `update operation failed`
              })
            }
            this.dialogVisible2 = false
            this.loadData()
          })
        } else {
          // console.log('the parameter is invalid')
          return false
        }
      })
    },

    deleteBrand(rowData){
      this.$confirm('Are you sure to delete the record?', 'Record Delete', {
        confirmButtonText: 'Confirm',
        cancelButtonText: 'Cancel',
        type: 'warning'
      }).then(() => {
        this.$store.dispatch('DeleteBrand',rowData.brd_id).then((result) => {
          if (result.code==200){
            this.$message({
              type: 'info',
              message: `delete operation succeeded`
            })
          }else{
            this.$message({
              type: 'info',
              message: `delete operation failed`
            })
          }
          this.loadData()
        })
      }).catch(() => {
      });
    },
    // ,
    //  resetForm(formName) {
    //   this.$refs[formName].resetFields()
    // }
    handleRemove(file, fileList) {
      // console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      // console.log(file)
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handleUploadSuccess(result) {
      // console.log(result)
      if(result.code != 200){
        this.$message({
          type: 'error',
          message: `upload failed`
        })
      }
      this.dialogImageUrl = result.data.uri
      this.img_id = result.data.img_id
    }
  },

}
</script>


<style scoped>
  #wishListDetailContainer {
    margin: 20px 20px 20px 20px
  }


  .ProductTable {
    margin-top: 50px;
    margin-left: 80px;
    margin-right: 80px;
  }

.BrandTable {
    margin-top: 50px;
    margin-left: 80px;
    margin-right: 80px;
  }

.divcss5{ color:#F00}

</style>
