<template>
  <div class="hello">
    <input type="file" @change="onChange" title="选择excel表">
  </div>
</template>

<script>
import XLSX from 'xlsx';
export default {
  name: 'Language',
  data() {
    return {
      langList: [], // 存储excel变量名数据
      cnList: [], // 存储excel中文数据
      cnjsList: [], // 存储excel中文解释数据
      enList: [], // 存储excel英文数据
      viList: [], // 存储excel越南语数据
      thList: [], // 存储excel泰国语数据
      cnObj: {}, // 存储最终中文数据
      cnjsObj: {}, // 存储最终中文解释数据
      enObj: {}, // 存储最终英文数据
      viObj: {}, // 存储最终越南语数据
      thObj: {}, // 存储最终越南语数据
    }
  },
  methods: {
    onChange(event) {
      var file = event.target.files[0];
      var reader = new FileReader();
      reader.onload = (event) => {
            var data = event.target.result;
            var workbook = XLSX.read(data, {type: 'binary'});
            this.outputWorkbook(workbook)
      }
      reader.readAsBinaryString(file);
    },
    // 读取 excel文件
    outputWorkbook(workbook) {
        var sheetNames = workbook.SheetNames; // 工作表名称集合
        sheetNames.forEach(name => {
            var worksheet = workbook.Sheets[name]; // 只能通过工作表名称来获取指定工作表
            for(var key in worksheet) {
                // v是读取单元格的原始值
                if (key, key[0] !== '!') {
                  // 默认A列为多语言变量名，B为中文、C为英文
                  if(key.substring(0,1) === 'A'){
                    this.langList.push(worksheet[key].v)
                  }else if(key.substring(0,1) === 'B'){
                    this.cnList.push(worksheet[key].v)
                  }else if(key.substring(0,1) === 'C'){
                    this.cnjsList.push(worksheet[key].v)
                  }else if(key.substring(0,1) === 'D'){
                    this.enList.push(worksheet[key].v)
                  }else if(key.substring(0,1) === 'E'){
                    this.viList.push(worksheet[key].v)
                  }else if(key.substring(0,1) === 'F'){
                    this.thList.push(worksheet[key].v)
                  }
                }
            }
        });
        this.produceJSON()
    },
    // 数组合成json
    produceJSON(){
      this.cnList.map((item,index)=>{
        // item.replcace(/\r/g, '')
        this.cnObj[this.langList[index]]=item
      })
      // this.cnjsList.map((item,index)=>{
      //   this.cnjsObj[this.langList[index]]=item
      // })
      this.enList.map((item,index)=>{
        this.enObj[this.langList[index]]=item
      })
      this.viList.map((item,index)=>{
        this.viObj[this.langList[index]]=item
      })
      this.thList.map((item,index)=>{
        this.thObj[this.langList[index]]=item
      })

      this.saveJSON(this.cnObj,'zh-CN')
      // 解释中文
      // this.saveJSON(this.cnjsObj,'zh-CNjs')
      // this.saveJSON(this.enObj,'en-US')
      // this.saveJSON(this.enObj,'en-IN')
      // this.saveJSON(this.viObj,'vi')
      // this.saveJSON(this.thObj,'th')
    },
    // 输出js文件
    saveJSON(data, filename){
      if(!data) {
        alert('保存的数据为空');
        return;
      }
      if(!filename) 
        filename = 'json.js'
      if(typeof data === 'object'){
        data = JSON.stringify(data, undefined, 4)
      }
      var blob = new Blob(['export default ' + data], {type: 'text/javascript'}),
      e = document.createEvent('MouseEvents'),
      a = document.createElement('a')
      a.download = filename
      a.href = window.URL.createObjectURL(blob)
      a.dataset.downloadurl = ['text/javascript', a.download, a.href].join(':')
      e.initMouseEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null)
      a.dispatchEvent(e)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
