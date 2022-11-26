<script setup>
import JabcodeJSInterface from './components/jabcodeJSLib.min.js'
import { ref } from 'vue'
import {Base64} from 'js-base64'
const imgStr=ref('');
const imgdec =ref('');
const text = ref('');
function codegenerator() {
  var jabcode=new JabcodeJSInterface();

  var textbase64=Base64.encode(text.value);
  imgStr.value=jabcode.encode_message(textbase64)
 

  jabcode.decode_message(imgStr.value).then(res=>{
    imgdec.value=Base64.decode(res);
 
  });
 
}

</script>

<template>
 <header>
 <h1>测试JABCode</h1>

 <div>鹿角加密研究工作室</div>
<br/>
<button @click="codegenerator">生成</button>
<div>加密内容：</div>
<textarea v-model="text" style="width: 80%;height: 40%;" placeholder="请输入要加密的内容"></textarea>
<br/>
<div>识别结果：</div>
<textarea style= "width: 80%;height: 40%;">{{imgdec}}</textarea>
<br/>
  <img :src="imgStr" alt="图片未生成" />
  <br/>

</header>
</template>


