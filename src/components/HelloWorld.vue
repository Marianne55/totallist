<template>
 <div>
   <Form ref="formCustom" :model="formCustom" :rules="ruleCustom" :label-width="80" style="border-style:solid">
        <FormItem label="title" prop="title">
            <Input type="text" v-model="formCustom.title"></Input>
        </FormItem>
        <FormItem label="state" prop="state">
            <Input type="text" v-model="formCustom.state"></Input>
        </FormItem>
        <FormItem label="data" prop="data">
            <Input type="text" v-model="formCustom.data" number></Input>
        </FormItem>
        <FormItem>
            <Button type="primary" @click="handleSubmit('formCustom')">Submit</Button>
            <Button @click="handleReset('formCustom')" style="margin-left: 8px">Reset</Button>
        </FormItem>
    </Form>
    <br/>
    <FlexTable 
        :loading="loading" 
        :columns="columns" 
        :data="list"
        @on-selection-change="onSelectionChange"
        @on-selection-cancel="onSelectionCancel"
        @on-all-cancel="onAllCancel">
        <template slot-scope="{ row, index }" slot="operation">
            <Button @click="show(row,index)">View</Button>
            <Button @click="remove(index)">Delete</Button>
        </template>
    ></FlexTable>
   <Modal
     title="修改"
     v-model="handleModal"
     @on-ok="ok"
  >
    <Form ref="formValidate" :model="formValidate" :label-width="80">
      <FormItem label="title" prop="title">
            <Input type="text" v-model="formValidate.title"></Input>
        </FormItem>
        <FormItem label="state" prop="state">
            <Input type="text" v-model="formValidate.state"></Input>
        </FormItem>
        <FormItem label="data" prop="data">
            <Input type="text" v-model="formValidate.data" number></Input>
        </FormItem>
    </Form>
   </Modal>
   <br/>
   <br/>
   <br/>
   <Input search enter-button="search" @on-search="search" v-model="value" placeholder="请输入标题查询"  />
   <br/>
   <FlexTable 
        :loading="loading" 
        :columns="columns1" 
        :data="sea"
        @on-selection-change="onSelectionChange"
        @on-selection-cancel="onSelectionCancel"
        @on-all-cancel="onAllCancel">
    ></FlexTable>
</div>
      
</template>
<script>
import { FlexTable } from 'tm-flextable';
export default {
    components:{
         FlexTable 
     },
    data(){
        return {
           index : 0,
           value :'',
           handleModal: false,
            columns: [
                {
                    type: 'selection',
                    width: 20,
                    align: 'center',
                    fixed: 'left'
                },
                {
                    title: '标题',
                    key: 'title',
                },
                {
                    title: '状态',
                    key: 'state',
                    render(h, params){
                        return h('span', params.row.state)
                    }
                },
               
                {
                    title: 'Date',
                    key: 'data',
                },
                {
                    title: '操作',
                    key: 'operation',
                    type: 'slot',
                },
            ],
            columns1: [
                {
                    type: 'selection',
                    width: 20,
                    align: 'center',
                    fixed: 'left'
                },
                {
                    title: '标题',
                    key: 'title',
                },
                {
                    title: '状态',
                    key: 'state',
                    render(h, params){
                        return h('span', params.row.state)
                    }
                },
               
                {
                    title: 'Date',
                    key: 'data',
                },
            ],
            loading: false,
            list:[],
            sea: [],
            formCustom: {
                    title: '',
                    state: '',
                    data: ''
                },
            formValidate: {
                    title: '',
                    state: '',
                    data: ''
                },
            formSearch: {
                    title: '',
                    state: '',
                    data: ''
                },
            ruleCustom: {

            }
        }
    },
    mounted() {},
    methods: {
        onSelectionChange(selection, row) {
            console.log('onSelectionChange', selection, row);
        },
        onSelectionCancel(row) {
            console.log('onSelectionCancel', row);
        },
        onAllCancel(cancelSelection) {
            console.log('onAllCancel', cancelSelection);
        },
        handleSubmit (name) {
                    this.$Message.success('Success!');
                    var params = JSON.parse(JSON.stringify(this.formCustom))
                    this.list.push(params)
          },
         show(row,index) {
            this.handleModal = !this.handleModal
            this.formValidate.title = row.title
            this.formValidate.state = row.state
            this.formValidate.data = row.data
            this.index = index
            

        },
         remove(index) {
            this.list.splice(index, 1)
        },
         ok(){
           
            this.list[this.index].title = this.formValidate.title
            this.list[this.index].state = this.formValidate.state
            this.list[this.index].data = this.formValidate.data
         },
         search(value){
             this.sea = []
             var that = this.value
             var target =this.list.filter(function(x){
                 return x.title.indexOf(that) > -1
             })
             console.log(target)
            this.sea.push(...target)              
             
         },
    }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
