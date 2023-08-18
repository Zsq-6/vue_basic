<script>
import { reactive, ref } from 'vue'
import axios from 'axios'
export default {
    setup() {
        // 定义表单数据对象
        const formData = reactive({
            username: '',
            password: ''
        })
        // 定义表单验证规则
        const formRules = reactive({
            username: [
                { required: true, message: '请输入用户名', trigger: 'blur' },
                { pattern: /^[a-z0-9]{5,10}$/, message: '用户名必须是5到10位的字母或数字', trigger: 'blur' }
            ],
            password: [
                { required: true, message: '请输入密码', trigger: 'blur' },
                { pattern: /^[a-zA-Z0-9]{6,12}$/, message: '密码必须是6到12位的字母或数字', trigger: 'blur' }
            ]
        })
        // 获取表单实例
        const formRef = ref(null)
        // 提交表单
        const submitForm = () => {
            formRef.value.validate(valid => {
                if (valid) {
                    // 表单验证通过，执行提交逻辑
                    axios.post('/api/login', formData)
                        .then(res => {
                            // 处理响应结果
                            console.log(res.data)
                        })
                        .catch(err => {
                            // 处理错误信息
                            console.log(err)
                        })
                } else {
                    // 表单验证失败，提示错误信息
                    alert('请检查表单输入')
                    return false
                }
            })
        }
        return {
            formData,
            formRules,
            formRef,
            submitForm
        }
    }
}
</script>
