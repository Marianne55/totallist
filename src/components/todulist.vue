<template>
 <div>
   <div class="topBody">
 <Form>
<Row>
<Col span="5">
<FormItem label="企业名称" class="mleft">
<Select style="width:230px;margin-bottom:20px;" v-model="companyName">
<Option value="乐山市公共交通总公司01">乐山市公共交通总公司01</Option>
<Option value="乐山市公共交通总公司02">乐山市公共交通总公司02</Option>
</Select>
</FormItem>
</Col>
<Col span="5">
<span class="input-label">车牌号码：</span>
<Input type="text" class="search"></Input>
</Col>
<Col span="14">
<FormItem>
<Button type="primary" @click="search">查询</Button>
<!-- <Button type="primary" style="margin-left:15px;">导出</Button> -->
<Button class="search-btn" type="primary" @click="addBus">
<Icon type="search"/>&nbsp;&nbsp;新增
</Button>
</FormItem>
</Col>
</Row>
</Form>
</div>
<div class="tableText">
<Table :columns="columns" :data="newList" border/>
</div>
<Modal
:title="modalTitle"
v-model="handleModal"
:footer-hide="true"
:mask-closable="false"
@on-visible-change="handleReset('formValidate')"
>
<Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
<FormItem label="企业名称" prop="name">
<Input v-model="formValidate.name" placeholder="请输入企业名称"></Input>
</FormItem>
<FormItem label="车牌号码" prop="carNumber">
<Input v-model="formValidate.carNumber" placeholder="请输入车牌号码"></Input>
</FormItem>
<FormItem>
<Button type="primary" @click="handleSubmit('formValidate')">提交</Button>
<Button @click="handleReset('formValidate')" style="margin-left: 8px">重置</Button>
</FormItem>
</Form>
</Modal>
</div>
</template>

<script>

export default {
name: 'todulist',
data() {
return {
handleModal: false,
modalTitle: '',
newList:[],
companyName: '',
formValidate: {
name: '',
carNumber: ''
},
ruleValidate: {
name: [
{
required: true,
message: '企业名称不能为空',
trigger: 'blur'
}
],
carNumber: [
{
required: true,
message: '车牌号码不能为空',
trigger: 'blur'
}
]
},
columns: [
{

title: '编号',

type: 'index',

width: 60,

align: 'center'

},

{

title: '企业名称',

key: 'name'

},

{

title: '车牌号',

key: 'carNumber'

},

{

title: '操作',

key: 'action',

width: 150,

align: 'center',

render: (h, params) => {

return h('div', [

h(

'Button',

{

props: {

type: 'primary',

size: 'small'

},

style: {

marginRight: '5px'

},

on: {

click: () => {

this.editBus(params.row, params.index)

}

}

},

'编辑'

),

h(

'Button',

{

props: {

type: 'error',

size: 'small'

},

on: {

click: () => {

this.remove(params.index)

}

}

},

'删除'

)

])

}

}

],

tableData: [

{

name: '乐山市公共交通总公司01',

age: 18,

carNumber: '川L00001',

date: '2016-10-03'

},

{

name: '乐山市公共交通总公司02',

age: 24,

carNumber: '川L00002',

date: '2016-10-01'

}

]

}

},

//在判断与select下拉框的值对应后，查询到的新的数据数组用push的方法，添加到newlist里面

methods: {

search() {

var len = this.tableData

var arr = []

for (var i in len) {

if (len[i].name == this.companyName) {

arr.push(len[i])

} else if (this.companyName == '') {

this.newList = this.tableData

return

}

}

this.newList = arr

},

// 增加用户时使用push方法，把用户的信息添加到list数组的最后

addBus() {

this.handleModal = true

this.modalTitle = '新增'

},

editBus(item, index) {

this.handleModal = true

this.modalTitle = '修改'

this.itemIndex = index

this.formValidate = JSON.parse(JSON.stringify(item))

},

handleSubmit(name) {

var self = this

self.$refs[name].validate(valid => {

if (valid) {

var params = JSON.parse(JSON.stringify(self.formValidate))

if (self.modalTitle === '新增') {

// 获取需要渲染到页面中的数据

self.$Message.success('新增成功!')

self.newList.push(params)

} else {

// 获取需要渲染到页面中的数据

this.$set(self.newList, self.itemIndex, params)

self.$Message.success('修改成功!')

}

self.handleModal = false

} else {

if (self.modalTitle === '新增') {

self.$Message.error('新增失败!')

} else {

self.$Message.error('修改失败!')

}

}

})

},

handleReset(name) {

this.$refs[name].resetFields()

},

//删除用户时，通过splice(index, 1)，可以删除index位置的数据，页面上的数据自动就会更新

remove(index) {

this.newList.splice(index, 1)

}

},

mounted() {

this.newList = this.tableData

}

}

</script>

 

<style>

.handle-box {

text-align: right;

margin-bottom: 10px;

}

.topBody {

width: 100%;

background-color: #ffffff;

padding-top: 30px;

}

.tableText {

width: 100%;

}

.mleft {

margin-left: 10px;

}

.search-btn{

float:right;

margin-right: 10px;

}

.input-label {

display: inline-block;

line-height: 16px;

margin-right: 15px;

font-size: 16px;

}

.search {

width: 230px;

}

</style>
