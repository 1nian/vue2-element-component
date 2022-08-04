<template>
  <div>
    <el-upload
      action="#"
      list-type="picture-card"
      :limit="limit"
      :auto-upload="false"
      :show-file-list="true"
      :before-upload="beforeAvatarUpload"
      :http-request="handleUpload"
      :on-remove="handleRemove"
      :on-change="handleChange"
      :file-list="list"
    >
      <div>
        <i slot="default" class="el-icon-plus"></i>
        <div slot="tip" class="el-upload__tip">限制上传{{ limit }}张</div>
      </div>
    </el-upload>
  </div>
</template>

<script>
export default {
  props: {
    // 限制最多图片数
    limit: {
      type: Number,
      default: 5
    },
    // 后端返回的图片列表
    fileLists: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      list: []
    }
  },
  mounted() {
    this.list = this.listProp
  },
  computed: {
    listProp() {
      return this.fileLists.map((item, index) => {
        return {
          url: item,
          uid: Date.now() + index++
        }
      })
    }
  },
  methods: {
    // 上传之前判断类型
    beforeAvatarUpload(file) {
      const typeLists = ['image/jpeg', 'image/png']
      const isType = typeLists.includes(file.type)
      const isLt2M = file.size / 1024 / 1024 < 2

      if (!isType) {
        this.$message.error('请上传png 或 jpg格式的图片!')
      }
      if (!isLt2M) {
        this.$message.error('图片大小不能超过 2MB!')
      }
      return isType && isLt2M
    },
    // 上传之前判断最大上传数量
    handleExceed() {
      if (this.list.length >= this.limit) {
        this.$message.warning(`最多上传${this.limit}张`)
        return false
      } else {
        return true
      }
    },
    // 图片删除
    handleRemove(file) {
      const uid = file.uid
      const listsIndex = this.list.findIndex(item => item.uid === uid)

      this.list.splice(listsIndex, 1)
      this.setImgLists()
    },
    // 上传
    handleChange(file) {
      this.handleUpload(file)
    },
    async handleUpload(file) {
      const params = new FormData()
      params.append('files', file.raw)
      try {
        if (this.handleExceed()) {
          // 通过后台接口获取返回的地址
          const info = {
            uid: file.uid || Date.now(),
            url : '上传成功之后后台返回的地址'
          }
          // 成功之后 push 到数组中
          this.list.push(info)

          // 成功之后触发
          this.setImgLists()
        }
      } catch (error) {
        console.log(error)
      }
    },
    setImgLists() {
      const lists = this.list.map(item => item.url)

      // 成功之后触发 getImgLists 将 lists 数组传递给父组件
      this.$emit('getImgLists', lists)
    }
  }
}
</script>
<style>
</style>
