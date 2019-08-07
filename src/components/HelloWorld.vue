<template>
 <div>
   <Form ref="formCustom" :model="formCustom" :rules="ruleCustom" :label-width="80">
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
    <FlexTable 
        :loading="loading" 
        :columns="columns" 
        :data="list"
        @on-selection-change="onSelectionChange"
        @on-selection-cancel="onSelectionCancel"
        @on-all-cancel="onAllCancel">
        <template slot-scope="{ row, index }" slot="operation">
            <Button @click="handleModal = true">View</Button>
            <Button @click="remove(index)">Delete</Button>
        </template>
    ></FlexTable>
   <Modal
     title="修改"
     v-model="handleModal"
  >
    <Form ref="formValidate" :model="formCustom" :label-width="80">
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
        <Button type="primary" @click="handleSubmit('formValidate')">提交</Button>
      </FormItem>
    </Form>
   </Modal>
</div>
</template>
<script>
import { FlexTable } from 'tm-flextable';
const aTestList = [];

    const oTestData = {
        title: '完成表格学习',
        state: '未完成',
        data: '2016-10-03',
    };
    aTestList.push(oTestData);

export default {
    components:{
         FlexTable 
     },
    data(){
        return {
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
            loading: false,
            list: aTestList,
            formCustom: {
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
                this.$refs[name].validate((valid) => {
                    if (valid) {
                        this.$Message.success('Success!');
                         var params = JSON.parse(JSON.stringify(this.formCustom))
                         aTestList.push(params)
                    } else {
                        this.$Message.error('Fail!');
                          }
                     })
          },
         show(index) {
            alert('show ' + index);
        },
         remove(index) {
            aTestList.splice(index, 1)
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
