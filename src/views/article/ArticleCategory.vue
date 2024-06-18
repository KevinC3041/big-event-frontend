<script setup>
    import {
        Edit,
        Delete
    } from '@element-plus/icons-vue'
    import { ref } from 'vue'

    const categories = ref([
        {
            "id": 3,
            "categoryName": '美食',
            "categoryAlias": "my",
            "createTime": "2023-09-02 12:06:59",
            "updateTime": "2023-09-02 12:06:59"
        }, 
        {
            "id": 4,
            "categoryName": "娱乐",
            "categoryAlias": "yl",
            "createTime": "2023-09-02 12:08:16",
            "updateTime": "2023-09-02 12:08:16"
        },
        {
            "id": 5,
            "categoryName": "军事",
            "categoryAlias": "js",
            "createTime": "2023-09-02 12:08:33",
            "updateTime": "2023-09-02 12:08:33"
        }
    ])
    // 声明一个异步函数
    import { articleCategoryListService, articleCategoryAddService } from '@/api/article.js'
    const articleCategoryList = async() => {
        let result = await articleCategoryListService();
        categories.value = result.data;
    }
    articleCategoryList();

    // 控制添加分类弹窗
    const dialogVisible = ref(false)

    // 添加分类数据模型
    const categoryModel = ref({
        categoryName: '',
        categoryAlias: ''
    })

    // 添加分类表单校验
    const rules = {
        categoryName: [
            { required: true, message: '请输入分类名称', trigger: "blur" },
            // { min:1,max:10,message:'长度为1~10位非空字符',trigger:'blur' }
        ],
        categoryAlias: [
            { required: true, message: '请输入分类别名', trigger: 'blur' },
            // { min:1,max:15,message:'长度为1~15位非空字符',trigger:'blur' }
        ]
    }

    // 调用接口，添加表单
    import {ElMessage} from "element-plus"
    const addCategory = async () => {
        let result = await articleCategoryAddService(categoryModel.value);
        ElMessage.success(result.message!=='操作成功' ? result.message : '添加成功')

        // 调用获取所有文章分类的函数
        articleCategoryList();
        dialogVisible.value = false;        
    }

</script>

<template>
    <el-card class="page-container">
        <template #header>
            <div class="header">
                <span>文章分类</span>
                <div class="extra">
                    <el-button type="primary" @click="dialogVisible=true">
                        添加分类
                    </el-button>
                </div>
            </div>
        </template>
        <el-table :data="categories" style="width:100%">
          <el-table-column label="序号" width="100" type="index"> </el-table-column>
          <el-table-column label="分类名称" prop="categoryName"></el-table-column>
          <el-table-column label="分类别名" prop="categoryAlias"></el-table-column>
          <el-table-column label="操作" width="100">
            <template #default="{ row }">
                <el-button :icon="Edit" circle plain type="primary">
                </el-button>
                <el-button :icon="Delete" circle plain type="danger">
                </el-button>
            </template>
          </el-table-column>
          <template #empty>
            <el-empty description="没有数据" />
          </template>
        </el-table>

        <!-- 添加分类弹窗 -->
        <el-dialog v-model="dialogVisible" title="添加弹层" width="30%">
            <el-form :model="categoryModel" :rules="rules"
            label-width="100px" style="padding-right: 30px">
                <el-form-item label="分类名称" prop="categoryName">
                    <el-input v-model="categoryModel.categoryName" minlength="1" maxlength="10">
                    <!-- <el-input v-model="categoryModel.categoryName"> -->
                    </el-input>
                </el-form-item>
                <el-form-item label="分类别名" prop="categoryAlias">
                    <el-input v-model="categoryModel.categoryAlias" minlength="1" maxlength="15">
                    <!-- <el-input v-model="categoryModel.categoryAlias"> -->
                    </el-input>
                </el-form-item>
            </el-form>
            <template #footer>
                <span class="dialog-footer">
                    <el-button @click="dialogVisible = false">取消</el-button>
                    <el-button type="primary" @click="addCategory">确认</el-button>
                </span>

            </template>

        </el-dialog>
    </el-card>
</template>

<style lang="scss" scoped>
    .page-container {
        min-height: 100%;
        /** ensure the item sets in the outer container, 
        i.e. clean the scroll bar */ 
        box-sizing: border-box;

        .header {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
    }
</style>