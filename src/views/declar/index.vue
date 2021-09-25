<template>
  <div class="app-container">
    <div class="steps">
      <el-steps :active="active" align-center >
        <el-step title="项目基本信息">
          <span slot="icon"> <i class="el-icon-edit" /> </span></el-step>
        <el-step title="项目资金信息">
          <span slot="icon"> <i class="el-icon-money" /> </span></el-step>
        <el-step title="项目绩效指标">
          <span slot="icon"> <i class="el-icon-tickets" /> </span></el-step>
      </el-steps>
    </div>

    <project-info
    ref='pojectInfoForm'
      v-show="active == 0"
      @handle-project-type="handleProjectType"
    />
    <money-info ref='moneyInfoForm' v-show="active == 1" :pro-type="projectType" />
    <project-indicator ref='projectIndicForm' v-show="active == 2" />

    <div class="btns">
      <el-button @click="goBack">返回</el-button>
      <el-button type="primary" v-if='!isDetail' @click='onSave'>暂存</el-button>
      <el-button
        v-if="active > 0"
        type="primary"
        plain
        @click="prev"
      >上一步</el-button>
      <el-button
        v-if="active < 2"
        type="success"
        @click="next"
      >下一步</el-button>
      <el-button v-if="active === 2 && !isDetail" type="success" @click="download">下载申报表</el-button>
      <el-button v-if="active === 2 && !isDetail" type="success" @click='onSubmit'>提交</el-button>
    </div>
  </div>
</template>

<script>
import moneyInfo from './components/money-info.vue'
import projectInfo from './components/project-info.vue'
import projectIndicator from './components/project-indicator.vue'
export default {
  components: {
    moneyInfo,
    projectInfo,
    projectIndicator
  },
  computed: {
    isDetail() {
      return this.$route.query.detail
    }
  },
  data() {
    return {
      active: 0,
      projectType: '',
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }
    }
  },
  methods: {
    //发ajax保存请求
    save(data) {
      //TODO伪代码
      return new Promise((resolve,reject)=>{
        resolve({data:{code:2000}})
           
      })
      /*
      return new Promise((resolve,reject)=>{
        axios.post({url:url,data:data}).then((res)=>{
          if(res.data.code==2000) {
            reslove(res)
          } else {
            reject()
          }
        }).catch(e=>reject(e))
      })
      */
    },
    onSave() {
      if(this.active == 0 && this.saveValidate() && !this.checkProjectNameIsRepeat() ) { //步骤1，2
        //TODO 暂存请求
        //TODO 返回列表页
        this.save().then(res=>{
          if(res.data.code) {
            this.$message({message:'保存成功', type:'success'})
            setTimeout(()=>{
              this.goBack()
            }, 2000)
          }
        })
      }
      if(this.saveValidate()) { //步骤3
        //TODO 暂存请求
        //TODO 返回列表页
        this.save().then(res=>{
          if(res.data.code) {
            this.$message({message:'保存成功', type:'success'})
            setTimeout(()=>{
              this.goBack()
            }, 2000)
          }
        })
      }
    },
    /**
     * @name: 检测项目名称是否重复
     * @return: 重复返回false
     */
    checkProjectNameIsRepeat() {
      //TODO发送请求 检测项目名称是否重复
      return false
    },
    /**
     * @name: 校验表单
     * @return: 校验失败返回false 成功为true
     */
    saveValidate() {
      if(this.active == 0) {
          let preojectInform = this.$refs.pojectInfoForm.form;
          if(!preojectInform.proType) {
              this.$message({type:'error',message:'项目类型必填'});
              return false
          }
          if(!preojectInform.proName) {
              this.$message({type:'error',message:'项目名称必填'});
              return false
          }
          if(!preojectInform.protNature) {
              this.$message({type:'error',message:'项目性质必填'})
              return false
          }
      } else if(this.active == 1){
        
      }
      return true
    },
    download() {
      if(this.validateAllForm()) {
        //TODO提交请求
      } 
    },
    onSubmit () {
      if(this.validateAllForm()) {
        //TODO提交请求
      } 
    },
    //校验所有表单
    async validateAllForm() {
        let validateForm = form => {
          return new Promise((resolve,reject)=>{
            form.validate(valid=>{
              if(valid) {
                resolve(valid)
              }else {
                reject(valid)
              }
            })
          })
        }
         
        //  三个阶段的表单
        let formValiPromiseArray = []
        for(var key in this.$refs) {
          let form = this.$refs[key].$refs[key]
          let formValidate= validateForm(form);
          formValiPromiseArray.push(formValidate)
        }

        //项目资金信息中的所有的表格中的表单
        if(this.$refs.moneyInfoForm) {
          for(var key in this.$refs.moneyInfoForm.$refs) {
            if(key !== 'moneyInfoForm') {
              debugger
                let form = this.$refs.moneyInfoForm.$refs[key].$refs[key]
                console.log(this.$refs.moneyInfoForm.$refs[key])
                let formValidate= validateForm(form);
                formValiPromiseArray.push(formValidate)
            }
          }
        }
  
        let p = await Promise.all(formValiPromiseArray).then(res=>res).catch(e => console.log(e));
        if(!Array.isArray(p) || p.some(bool => !bool)) {
          this.$message({message:'请填写必填项',type:'error'})
          return false
        } else {
          return true
        }
        
       
    },
    next() {
      this.active++
      return
      debugger
      if (this.active < 2 && this.active >= 0) {
        if(this.active == 0 && this.saveValidate() && !this.checkProjectNameIsRepeat()) { //阶段1的下一步
          //TODO 暂存请求
          this.save().then((res)=>{
            if(res.data.code) {
              this.$message({message:'保存成功', type:'success'})
              this.active++
            }
          })
        }
        if(this.active == 1 && this.saveValidate() ) {//阶段2的下一步
          //TODO 暂存请求
          this.save().then((res)=>{
            if(res.data.code) {
              this.$message({message:'保存成功', type:'success'})
              this.active++
            }
          })
        }
      }
    },
    prev() {
      if (this.active > 0) {
        this.active--
      }
    },
    handleProjectType(v) {
      this.projectType = v
    },
    goBack() {
      this.$router.push({ path: 'mydeclar' })
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
}
</script>

<style scoped lang="scss">
.line {
  text-align: center;
}
.steps {
  width: 40%;
  margin: 0 auto;
  margin-top: 28px;
}
.btns {
  position: absolute;
  left: 50%;
  transform: translate(-50%);
  bottom: 20px;
}
::v-deep .el-step.is-horizontal .el-step__line {
  height: 2px;
  top: 29px;
  left: 50%;
  right: -50%;
}
::v-deep .el-step__icon {
  width: 60px;
  height: 60px;
  font-size: 30px;
}
::v-deep .is-finish,::v-deep .is-,::v-deep .is-process,::v-deep .is-success {
  border-color: #4280e8 !important;
  color: #4280e8;
  font-weight: 700;
  .el-step__line {
    background-color: #4280e8 !important;
  }
  .el-step__icon {
     background: #4280e8;
     color:#fff;
   }
}

// ::v-deep .is- {
//   border-color: #4280e8 !important;
//   color:#4280e8;
//   .el-step__icon {
//      background: #4280e8;
//      color:#fff;
//    }
// }

// ::v-deep .is-success {
//   border-color: #4280e8 !important;
//   color:#4280e8;
//   .el-step__icon {
//      background: #4280e8;
//      color:#fff;
//    }
// }
// ::v-deep .is-process {
//   border-color: #4280e8 !important;
//   color:#4280e8;
//   .el-step__icon {
//      background: #4280e8;
//      color:#FFFf;
//    }
// }

 
</style>
