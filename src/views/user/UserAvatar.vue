<script setup>
  import { ref } from "vue";
  import { Plus, Upload } from "@element-plus/icons-vue";

  const imageUrl = ref("");
</script>

<template>
  <el-card class="page-container">
    <template #header>
      <div class="card-header">
        <span>更换头像</span>
      </div>
    </template>
    <el-row>
      <el-col :span="12" class="upload-col">
        <div class="upload-group">
          <!-- 
                    auto-upload:设置是否自动上传 
                    action:设置服务器接口路径
                    name:设置上传的文件字段名
                    header:设置上传的请求头
                    on-success:设置上传成功的回调函数
                -->
          <el-upload
            class="avatar-uploader"
            :auto-upload="true"
            :show-file-list="false"
            action=""
            name="file"
            :headers="{}"
          >
            <img v-if="imageUrl" :src="imageUrl" class="avatar" />
            <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
          </el-upload>
          <br />
          <div class="button-group">
            <el-button type="primary" :icon="Plus" size="large">选择图片</el-button>
            <el-button type="success" :icon="Upload" size="large">上传头像</el-button>
          </div>
        </div>
      </el-col>
    </el-row>
  </el-card>
</template>

<style lang="scss" scoped>

  .upload-col {
    // background-color: #ff0;
    display: flex;
    align-items: start;
  }

  .upload-group {
    // background-color: #0f0;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .button-group {
    width:100%;
    display: flex;
    justify-content: space-evenly;
    // background-color: #ff0;
  }

  .avatar-uploader {
    // background-color: #0f0;
    :deep() {
      .el-icon.avatar-uploader-icon {
        // background-color: #ff0;
        height: 278px;
        width: 278px;
        font-size: 28px;
        color: #8c939d;
        text-align: center;
      }
      .el-upload {
        // background-color: #ff0;
        border: 1px dashed var(--el-border-color);
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
        transition: var(--el-transition-duration-fast);
      }
      .el-upload:hover {
        border-color: var(--el-color-primary);
      }
    }
  }
</style>
