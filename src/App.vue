<script setup>
import JabcodeJSInterface from './components/jabcodeJSLib.min.js'

import { ref } from 'vue'
import {Base64} from 'js-base64'
import { ElMessage } from 'element-plus'

import {
  Check,
  Delete,
  Edit,
  Upload,
  Cpu,
  Star,
} from '@element-plus/icons-vue'
const imgStr=ref('');
const imgdec =ref('');
const text = ref('');
const viscode = ref(false);
const jabcode=new JabcodeJSInterface();
function jabencode() {
  
  if(text.value=="")
  {
  ElMessage({
    showClose: true,
    message: '输入内容不能为空',
    type: 'error',
  })
    return;
  }
  viscode.value=true;
  var textbase64=Base64.encode(text.value);
  imgStr.value=jabcode.encode_message(textbase64,25,64);
 //console.log( imgStr.value)
 // var srccode=textbase64.value.replace("data:image/png;base64,","");


 
}


function jabdecode(imgbase64)
{
  jabcode.decode_message(imgbase64).then(res=>{
    imgdec.value=Base64.decode(res);
    console.log(imgdec.value);
  });
}

function getFile(file, fileList) {
  getBase64(file.raw).then(res => {
    const params = res
    imgStr.value = params;
    console.log(params);
    jabdecode(params);
  })
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
function  handlePictureCardPreview(file) {

}

</script>

<template>
 <el-main>
  <el-card class="box-card">
 <h1>JAB彩虹码密信工具</h1>
 <el-row>
 <div>Create from AntlersLab</div>
 <el-link type="info" :icon="Edit" href="https://github.com/jsBrique/jabcode_vue_demo">开源代码</el-link>
</el-row>
 <el-divider />

 <el-row class="mb-4" :type="flex" >

<el-button type="primary"  :icon="Cpu" @click="jabencode">生成</el-button>
<!-- <el-button type="primary" :icon="Upload" @click="jabdecode">上传</el-button> -->

<div>
<el-upload
  list-type="picture"
   action=''
   accept=".jpg, .png"
   :limit="1"
   :auto-upload="false"
   :file-list="fileList"
   :on-change="getFile"
   :on-preview="handlePictureCardPreview"
   :on-remove="handleUploadRemove"
   >
	<el-button style="margin-left:10px;" type="primary" :icon="Upload"  @click="uploadimg">上传</el-button>
</el-upload>
</div>

</el-row>
<div>加密内容</div>

<el-input type="textarea"  v-model="text" style="resize:none; width: 80%;height: 40%;" placeholder="请输入要加密的内容"></el-input>

<div>识别结果：</div>
<el-input type="textarea" style= "resize:none; width: 80%;height: 40%;">{{imgdec}}</el-input>


<div v-if="viscode">
  <div>长按图片保存即可分享~</div>
  <img :src="imgStr" alt="图片未生成" />
  </div>
  

</el-card>
</el-main>
</template>


