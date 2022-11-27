<script setup>
import JabcodeJSInterface from './components/jabcodeJSLib.min.js'

import { ref } from 'vue'
import { Base64 } from 'js-base64'
import { ElMessage } from 'element-plus'

import {
  Check,
  Delete,
  Edit,
  Upload,
  Cpu,
  Star,
  Plus
} from '@element-plus/icons-vue'

const imgStr = ref('');
const imgsrc = ref('');
const imgdec = ref('');
const text = ref('');
const viscode = ref(false);
const jabcode = new JabcodeJSInterface();
const activeName = ref('scan');
function jabencode() {

  if (text.value == "") {
    ElMessage({
      showClose: true,
      message: '输入内容不能为空',
      type: 'error',
    })
    return;
  }
  viscode.value = true;
  var textbase64 = Base64.encode(text.value);
  imgStr.value = jabcode.encode_message(textbase64, 25, 64);



}


function jabdecode(imgbase64) {
  jabcode.decode_message(imgbase64).then(res => {
    imgdec.value = Base64.decode(res);

  });
}

function getFile(file, fileList) {
  getBase64(file.raw).then(res => {
    const params = res
    imgsrc.value = params;

    jabdecode(imgsrc.value);
  });


}

function getBase64(file) {
  return new Promise(function (resolve, reject) {
    const reader = new FileReader()
    let imgResult = ''
    reader.readAsDataURL(file)
    reader.onload = function () {
      imgResult = reader.result
    }
    reader.onerror = function (error) {
      reject(error)
    }
    reader.onloadend = function () {
      resolve(imgResult)
    }
  })
}


function handleUploadRemove(file, fileList) {

}
function handlePictureCardPreview(file) 
{

}

</script>

<template  class="wrap">

  <el-card class="box-card">
    <template #header>
      <h1>JAB彩虹码密信工具</h1>

      <el-row>
        <div>Create from dobriq</div>
        <el-link type="info" :icon="Edit" style="margin-left:10px;" href="https://github.com/jsBrique/jabcode_vue_demo">
          项目源码</el-link>
        <el-link type="info" :icon="Edit" style="margin-left:10px;" href="https://github.com/jabcode/jabcode">JABCode
        </el-link>
      </el-row>

    </template>
    <el-main>


      <el-tabs v-model="activeName" type="card">
        <el-tab-pane label="生成" name="gen">

          <el-button type="warning" :icon="Cpu" @click="jabencode">生成</el-button>
          <br />
          <br />
          <el-input type="textarea" resize="none" rows="5" v-model="text" width="80%" placeholder="请输入要加密的内容">
          </el-input>
          <div v-if="viscode">
        
            <br/>
            <img :src="imgStr" alt="图片未生成" />
            <br/>
            <div>长按图片保存即可分享~</div>
          </div>
        </el-tab-pane>
        <el-tab-pane label="识别" name="scan">
 
          <el-upload list-type="picture-card" action='' drag="true" accept=".jpg, .png" :limit="1" :auto-upload="false"
            :file-list="fileList" :on-change="getFile" :on-preview="handlePictureCardPreview"
            :on-remove="handleUploadRemove">
            <el-link :icon="Plus">
              导入
            </el-link>

          </el-upload>
          <div>识别文本：</div>
          <el-input type="textarea" resize="none" v-model="imgdec" rows="5" width="80%"></el-input>

        </el-tab-pane>

      </el-tabs>





    </el-main>
  </el-card>
  
</template>


<style>
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.box-card {
  min-width: 480px;
  max-width: 620px;
  align-items: center;
 

 
   
    
}
.warp {

  position: relative;


}
</style>