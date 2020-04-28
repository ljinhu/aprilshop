<template>
    <el-dialog
            :title="!dataForm.id ? '新增' : '修改'"
            :close-on-click-modal="false"
            :visible.sync="visible">
        <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
                                                                            <el-form-item label="路由id" prop="routeId">
                        <el-input v-model="dataForm.routeId" placeholder="路由id"></el-input>
                    </el-form-item>
                                                                <el-form-item label="uri路径" prop="uri">
                        <el-input v-model="dataForm.uri" placeholder="uri路径"></el-input>
                    </el-form-item>
                                                                <el-form-item label="判定器" prop="predicates">
                        <el-input v-model="dataForm.predicates" placeholder="判定器"></el-input>
                    </el-form-item>
                                                                <el-form-item label="过滤器" prop="filters">
                        <el-input v-model="dataForm.filters" placeholder="过滤器"></el-input>
                    </el-form-item>
                                                                <el-form-item label="排序" prop="orders">
                        <el-input v-model="dataForm.orders" placeholder="排序"></el-input>
                    </el-form-item>
                                                                <el-form-item label="描述" prop="description">
                        <el-input v-model="dataForm.description" placeholder="描述"></el-input>
                    </el-form-item>
                                                                <el-form-item label="状态：Y-有效，N-无效" prop="status">
                        <el-input v-model="dataForm.status" placeholder="状态：Y-有效，N-无效"></el-input>
                    </el-form-item>
                                                                <el-form-item label="创建时间" prop="createdTime">
                        <el-input v-model="dataForm.createdTime" placeholder="创建时间"></el-input>
                    </el-form-item>
                                                                <el-form-item label="更新时间" prop="updatedTime">
                        <el-input v-model="dataForm.updatedTime" placeholder="更新时间"></el-input>
                    </el-form-item>
                                                                <el-form-item label="创建人" prop="createdBy">
                        <el-input v-model="dataForm.createdBy" placeholder="创建人"></el-input>
                    </el-form-item>
                                                                <el-form-item label="更新人" prop="updatedBy">
                        <el-input v-model="dataForm.updatedBy" placeholder="更新人"></el-input>
                    </el-form-item>
                                    </el-form>
        <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
    </el-dialog>
</template>

<script>
    export default {
        data () {
            return {
                visible: false,
                dataForm: {
                                                id: 0,
                                                                routeId: '',
                                                                uri: '',
                                                                predicates: '',
                                                                filters: '',
                                                                orders: '',
                                                                description: '',
                                                                status: '',
                                                                createdTime: '',
                                                                updatedTime: '',
                                                                createdBy: '',
                                                                updatedBy: ''
                                    },
            dataRule: {
                                                                                                routeId: [
                        { required: true, message: '路由id不能为空', trigger: 'blur' }
                    ],
                                                                                uri: [
                        { required: true, message: 'uri路径不能为空', trigger: 'blur' }
                    ],
                                                                                predicates: [
                        { required: true, message: '判定器不能为空', trigger: 'blur' }
                    ],
                                                                                filters: [
                        { required: true, message: '过滤器不能为空', trigger: 'blur' }
                    ],
                                                                                orders: [
                        { required: true, message: '排序不能为空', trigger: 'blur' }
                    ],
                                                                                description: [
                        { required: true, message: '描述不能为空', trigger: 'blur' }
                    ],
                                                                                status: [
                        { required: true, message: '状态：Y-有效，N-无效不能为空', trigger: 'blur' }
                    ],
                                                                                createdTime: [
                        { required: true, message: '创建时间不能为空', trigger: 'blur' }
                    ],
                                                                                updatedTime: [
                        { required: true, message: '更新时间不能为空', trigger: 'blur' }
                    ],
                                                                                createdBy: [
                        { required: true, message: '创建人不能为空', trigger: 'blur' }
                    ],
                                                                                updatedBy: [
                        { required: true, message: '更新人不能为空', trigger: 'blur' }
                    ]
                                                }
        }
        },
        methods: {
            init (id) {
                this.dataForm.id = id || 0
                this.visible = true
                this.$nextTick(() => {
                    this.$refs['dataForm'].resetFields()
                if (this.dataForm.id) {
                    this.$http({
                        url: this.$http.adornUrl(`/ware/gatewayroute/info/${this.dataForm.id}`),
                            method: 'get',
                params: this.$http.adornParams()
                }).then(({data}) => {
                        if (data && data.code === 0) {
                                                                                                            this.dataForm.routeId = data.gatewayRoute.routeId
                                                                                        this.dataForm.uri = data.gatewayRoute.uri
                                                                                        this.dataForm.predicates = data.gatewayRoute.predicates
                                                                                        this.dataForm.filters = data.gatewayRoute.filters
                                                                                        this.dataForm.orders = data.gatewayRoute.orders
                                                                                        this.dataForm.description = data.gatewayRoute.description
                                                                                        this.dataForm.status = data.gatewayRoute.status
                                                                                        this.dataForm.createdTime = data.gatewayRoute.createdTime
                                                                                        this.dataForm.updatedTime = data.gatewayRoute.updatedTime
                                                                                        this.dataForm.createdBy = data.gatewayRoute.createdBy
                                                                                        this.dataForm.updatedBy = data.gatewayRoute.updatedBy
                                                            }
                })
                }
            })
            },
            // 表单提交
            dataFormSubmit () {
            this.$refs['dataForm'].validate((valid) => {
                if (valid) {
                    this.$http({
                        url: this.$http.adornUrl(`/ware/gatewayroute/${!this.dataForm.id ? 'save' : 'update'}`),
                            method: 'post',
                data: this.$http.adornData({
                                                        'id': this.dataForm.id || undefined,
                                                                    'routeId': this.dataForm.routeId,
                                                                    'uri': this.dataForm.uri,
                                                                    'predicates': this.dataForm.predicates,
                                                                    'filters': this.dataForm.filters,
                                                                    'orders': this.dataForm.orders,
                                                                    'description': this.dataForm.description,
                                                                    'status': this.dataForm.status,
                                                                    'createdTime': this.dataForm.createdTime,
                                                                    'updatedTime': this.dataForm.updatedTime,
                                                                    'createdBy': this.dataForm.createdBy,
                                                                    'updatedBy': this.dataForm.updatedBy
                                                })
                }).then(({data}) => {
                        if (data && data.code === 0) {
                    this.$message({
                        message: '操作成功',
                                type: 'success',
                                duration: 1500,
                                onClose: () => {
                            this.visible = false
                        this.$emit('refreshDataList')
                        }
                    })
                    } else {
                    this.$message.error(data.msg)
                    }
                })
                }
            })
            }
        }
    }
</script>
