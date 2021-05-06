<template>
    <div>
        <Card>
            <div >
                <Form ref="formValidate" label-position="left" :model="formValidate" :rules="ruleValidate" :label-width="120">
                    <FormItem label="Title" prop="title" >
                        <Input v-model="formValidate.title" placeholder="道路"></Input>
                    </FormItem>
                    <FormItem label="Keywords" prop="keywords" >
                        <Input v-model="formValidate.keywords" placeholder="Keywords"></Input>
                    </FormItem>
                    <FormItem label="Description" prop="description">
                        <Input v-model="formValidate.description" type="textarea" :autosize="{minRows: 2}" placeholder="Enter  description..."></Input>
                    </FormItem>
                    <FormItem label="RecordNumber" prop="recordNumber" >
                        <Input v-model="formValidate.recordNumber" placeholder="RecordNumber"></Input>
                    </FormItem>
                            <Option  v-for="item in themes" :value="item" :key="item">{{ item }}</Option>
                        </Select>
                    </FormItem>
                    <FormItem>
                        <Button type="primary" @click="handleSubmit('formValidate')">Submit</Button>
                        <Button @click="handleReset('formValidate')" style="margin-left: 8px">Reset</Button>
                    </FormItem>
                </Form>


            </div>
        </Card>
    </div>
</template>

<script>
    import { SystemIndex,SystemUpdate} from '@/api/system'

    export default {
        data () {
            return {
                formValidate: {
                    title: '',
                    keywords: '',
                    description: '',
                    recordNumber: '',
                },
                ruleValidate: {
                    title: [
                        { required: true, message: 'The title cannot be empty', trigger: 'blur' },
                        { max:100, message: 'The title length is too long', trigger: 'blur'}
                    ],
                    keywords: [
                        { required: true, message: 'The keywords cannot be empty', trigger: 'blur' },
                        { max:100, message: 'The keywords length is too long', trigger: 'blur'}
                    ],
                    description: [
                        { required: true, message: 'The description cannot be empty', trigger: 'blur' },
                        { max:200, message: 'The description length is too long', trigger: 'blur'}
                    ],
                    recordNumber: [
                        { required: true, message: 'The recordNumber can not be empty', trigger: 'blur' },
                        { max:50, message: 'The recordNumber length is too long', trigger: 'blur'}
                    ],
                },
                systemId: 0,
            }
        },
        mounted() {
            this.defaultData();
        },
        methods: {
            defaultData () {
                SystemIndex().then(res => {
                    this.systemId = res.data.data.system.Id;
                    this.formValidate.title = res.data.data.system.Title;
                    this.formValidate.keywords = res.data.data.system.Keywords;
                    this.formValidate.description = res.data.data.system.Description;
                    this.formValidate.recordNumber = res.data.data.system.RecordNumber;
                }).catch(err => {
                    this.$Message.error("操作失败"+ err);
                })
            },
            handleSubmit (name) {
                let that = this;
                this.$refs[name].validate((valid) => {
                    if (valid) {
                        SystemUpdate(that.systemId,that.formValidate.title,that.formValidate.keywords,that.formValidate.description,that.formValidate.recordNumber,that.formValidate.theme)
                            .then(res => {
                                if (res.data.code === 0) {
                                    this.$Message.success(res.data.message);
                                    setTimeout(() => {
                                        this.$router.push('/backend/system/index')
                                    },2000)
                                } else {
                                    this.$Message.error(res.data.message);

                                }
                            }).catch(err => {
                            this.$Message.error("操作失败"+ err);
                        })
                    } else {
                        this.$Message.error('Fail!');
                    }
                })
            },
            handleReset (name) {
                this.$refs[name].resetFields();
            }
        }
    }
</script>
