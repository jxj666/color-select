<template>
  <div class="hello">
    <el-container>
      <el-header>
        <el-row :gutter="20">
          <el-col :span="6">
            <div class="grid-content ">
              <el-input placeholder="请输入起始颜色" v-model="color1" clearable>
              </el-input>
            </div>
          </el-col>
          <el-col :span="6">
            <div class="grid-content ">
              <el-input placeholder="请输入结束颜色" v-model="color2" clearable>
              </el-input>
            </div>
          </el-col>
          <el-col :span="6">
            <div class="grid-content ">
              <el-input placeholder="请输入阶段数" v-model="input3" clearable>
              </el-input>
            </div>
          </el-col>
          <el-col :span="6">
            <div class="grid-content f-t-r">
              <el-button type="warning" @click='clear'>清空</el-button>
              <el-button type="danger" @click='start'>开始</el-button>
            </div>
          </el-col>
        </el-row>
      </el-header>
      <el-container>
        <el-aside width="200px">
          <div class="block">
            <span class="demonstration">起始颜色</span>
            <el-color-picker v-model="color1"></el-color-picker>
          </div>
          <div class="block">
            <span class="demonstration">结束颜色</span>
            <el-color-picker v-model="color2"></el-color-picker>
          </div>
        </el-aside>
        <el-main>
          <template v-for="x in arr">
            <el-alert v-bind:title="x.text" type="success" v-bind:style="{ background: x.color }">
            </el-alert>
          </template>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      input1: '',
      input2: '',
      input3: '',
      color1: null,
      color2: null,
      arr: [{ text: '请输入参数!', color: '#eee' }]
    }
  },
  methods: {
    start: function() {
      var input3 = parseInt(this.input3) - 1 || 1
      var color1 = this.color1.substring(1)
      var color2 = this.color2.substring(1)

      function digital(color) {
        if (color.length === 3) {
          var arr = color.split('')
          color = `${arr[0]}${arr[0]}${arr[1]}${arr[1]}${arr[2]}${arr[2]}`
        } else if (color.length === 6) {} else {
          return false
        }
        var r = parseInt(color.substr(0, 2), 16)
        var g = parseInt(color.substr(2, 2), 16)
        var b = parseInt(color.substr(4, 2), 16)
        var rgb = [r, g, b]
        return rgb
      }
      var arr1 = digital(color1)
      var arr2 = digital(color2)
      if (!arr1 || !arr2) {
        alert('请输入正确参数!')
        return
      }
      var step1 = (arr1[0] - arr2[0]) / input3
      var step2 = (arr1[1] - arr2[1]) / input3
      var step3 = (arr1[2] - arr2[2]) / input3
      var list = []
      for (var i = 0; i < input3; i++) {
        var r = Math.round(arr1[0] - step1 * i)
        var g = Math.round(arr1[1] - step2 * i)
        var b = Math.round(arr1[2] - step3 * i)
        console.log(r, g, b)
        var obj = {}
        var r1 = r.toString(16)
        var g1 = g.toString(16)
        var b1 = b.toString(16)
        var r2 = ('00' + r1).substr(-2)
        var g2 = ('00' + g1).substr(-2)
        var b2 = ('00' + b1).substr(-2)
        obj.text = `#${r2}${g2}${b2}`
        obj.color = `#${r2}${g2}${b2}`
        list.push(obj)
      }
      var last = {}
      last.text = `#${arr2[0].toString(16)}${arr2[1].toString(16)}${arr2[2].toString(16)}`
      last.color = `#${arr2[0].toString(16)}${arr2[1].toString(16)}${arr2[2].toString(16)}`
      list.push(last)
      this.arr = list
      console.log(arr1, arr2, step1, step2, step3, list)
    },
    clear: function() {
      this.input3 = ''
      this.color1 = null
      this.color2 = null
      this.arr = [{ text: '请输入参数!', color: '#eee' }]
    }
  }
}

</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.el-header,
.el-footer {
  background-color: #B3C0D1;
  color: #333;
  text-align: center;
  line-height: 60px;
}

.el-aside {
  background-color: #D3DCE6;
  color: #333;
  text-align: center;
  line-height: 100px;
  min-height: 200px;
}

.el-main {
  background-color: #E9EEF3;
  color: #333;
  text-align: center;
  line-height: 20px;
}

.el-alert {
  line-height: 20px;
  margin: 10px 0;
  border: 1px solid #fff;
  text-shadow: 0 0 1px #fff;
  color: #000;
}

body>.el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
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

.bg-purple-dark {
  background: #99a9bf;
}

.bg-purple {
  background: #d3dce6;
}

.bg-purple-light {
  background: #e5e9f2;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}

.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}

.f-t-r {
  text-align: right;
}

.demonstration {
  display: block;
  line-height: 20px;
  margin-top: 20px;
}

.el-color-picker {
  line-height: 30px;
}

</style>
