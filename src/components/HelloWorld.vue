<template>
  <div id="HelloWorld">
    <el-container>
      <el-main>
        <el-button type @click="downloadImg('img')" size="mini" plain>下载图片</el-button>
      </el-main>
    </el-container>
  </div>
</template>
<script>
import FileSaver from "file-saver";
var JSZip = require("jszip");
export default {
  name: "HelloWorld",

  data() {
    return {};
  },

  methods: {
    downloadImg(blogTitle) {
      var zip = new JSZip();
      var imgs = zip.folder(blogTitle);
      var baseList = [];
      var _this = this;
      //this.$axios.post('http://192.168.88.6:9998/msg/getImgList', query, res => {

      let imgList = [
        "http://ci.xiaohongshu.com/934bfefa-1150-3677-83fc-efcb095230ba?imageView2/2/w/1080/format/webp",
        "http://ci.xiaohongshu.com/0e099e54-571c-53f0-8b55-0e7ff100aad9?imageView2/2/w/1080/format/webp",
        "http://ci.xiaohongshu.com/db6ce95e-dcb0-58d2-9fa2-083b589d1c38?imageView2/2/w/1080/format/webp",
        "http://ci.xiaohongshu.com/94cabc89-aaae-3608-bba3-c6a3d8098aa6?imageView2/2/w/1080/format/webp"
      ];

      // let imgList = [
      //   "http://wx2.sinaimg.cn/thumb150/0075osWcgy1ftgqyorm9mj315o15me82.jpg",
      //   "http://wx1.sinaimg.cn/thumb150/0075osWcgy1ftgqynincgj315o15me82.jpg",
      //   "http://wx4.sinaimg.cn/thumb150/0075osWcgy1ftgqypdlvsj315o15mgwv.jpg",
      //   "http://wx2.sinaimg.cn/thumb150/0075osWcgy1ftgqyqdsr7j32c0340b2b.jpg"
      // ];

      // let imgList=[
      // "https://auto-publish-content.oss-cn-shenzhen.aliyuncs.com/image/d7e34500-0462-4985-9edf-ef79783bb1e5.png?Expires=1555400752&OSSAccessKeyId=LTAIsMr6kbkFx5fK&Signature=nlpS%2BA4TPZrJD%2FIIcshTtSAt3bc%3D",
      // "https://auto-publish-content.oss-cn-shenzhen.aliyuncs.com/image/fa21c82f-f3a1-4add-93a1-4ea925171e25.png?Expires=1555400752&OSSAccessKeyId=LTAIsMr6kbkFx5fK&Signature=vafBqvCH6uA2%2Fnb%2FVv5VARXqXHA%3D",]

      let arr = imgList;
      for (var i = 0; i < arr.length; i++) {
        let image = new Image();
        // 解决跨域 Canvas 污染问题
        image.setAttribute("crossOrigin", "anonymous");
        image.onload = function() {
          let canvas = document.createElement("canvas");
          canvas.width = image.width;
          canvas.height = image.height;
          let context = canvas.getContext("2d");
          context.drawImage(image, 0, 0, image.width, image.height);
          let url = canvas.toDataURL(); // 得到图片的base64编码数据 let url = canvas.toDataURL('image/png')
          baseList.push(url.substring(22));
          if (baseList.length === arr.length) {
            if (baseList.length > 0) {
              _this.$notify({
                title: "成功",
                message: "即将下载",
                type: "success"
              });
              for (let k = 0; k < baseList.length; k++) {
                imgs.file("photo" + k + ".png", baseList[k], { base64: true });
              }
              zip.generateAsync({ type: "blob" }).then(function(content) {
                // see FileSaver.js
                saveAs(content, blogTitle + ".zip");
              });
            } else {
              _this.$notify.error({
                title: "错误",
                message: "暂无图片可下载"
              });
            }
          }
        };
        image.src = arr[i].replace("jpg", "png");
      }
      //})
    }
  }
};
</script>
<style  scoped>
</style>
