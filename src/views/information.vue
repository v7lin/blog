<template>
  <Container :open="loading">
    <h2 class="information-title">用户信息</h2>
    <mu-flex class="information-box">
      <mu-form :model="form">
        <div class="input-group">
          <div class="input-text">头像：</div>
          <!-- <img class="avatar" src="../assets/img/timg.jpg" alt="" v-show="!avatar"> -->
          <img :src="form.avatar" @click="selectImg" alt class="avatar" />
          <mu-button @click="selectImg" class="upload-btn" small v-show="!isChangeImage">上传</mu-button>
          <mu-button @click="onUploadConfirm" class="upload-btn" color="primary" small v-show="isChangeImage">确定</mu-button>
          <mu-button @click="onUploadCancel" class="upload-btn" small v-show="isChangeImage">取消</mu-button>
          <input @change="changeImage($event.target.files)" accept="image" name="avatar" ref="avatar" type="file" v-show="false" />
        </div>
        <div class="input-group">
          <div class="input-text">用户名：</div>
          <input class="input-value" name="username" placeholder="请输入用户名" type="text" v-model="form.username" />
        </div>
        <div class="input-group">
          <div class="input-text">电话</div>
          <input class="input-value" name="phone" placeholder="请输入电话" type="text" v-model="form.phone" />
        </div>
        <div class="input-group">
          <div class="input-text">邮箱：</div>
          <input class="input-value" name="email" placeholder="请输入邮箱" type="text" v-model="form.email" />
        </div>
        <div class="input-group">
          <mu-button @click="submit" color="primary" slot="actions">提交</mu-button>
        </div>
      </mu-form>
    </mu-flex>
  </Container>
</template>

<script>
import Container from '../components/container'
import commonUtil from '../utils/common'
import defaultAvatar from '../assets/img/timg.jpg'
import User from '../api/user'
export default {
  components: {
    Container
  },
  data () {
    return {
      usernameRules: [
        {validate: val => !!val, message: '必须填写用户名'},
        {validate: val => val.length >= 6, message: '用户名长度大于6'},
        {validate: val => !new RegExp("[`~!@#$^&*()=|{}':;',\\[\\]<>/?~！@#￥……&*（）——|{}【】‘；：”“'。，、？]").test(val), message: '用户名不允许使用特殊字符'}
      ],
      passwordRules: [{validate: val => !!val, message: '必须填写密码'}, {validate: val => val.length >= 6, message: '密码长度大于等于6'}],
      phoneRules: [{validate: val => !!val, message: '必须填写手机号'}, {validate: val => /(^1[3|5|8][0-9]{9}$)/.test(val) || /^0\d{2,3}-?\d{7,8}$/.test(val), message: '手机号输入有误'}],
      emailRules: [{validate: val => !!val, message: '必须填写邮箱'}, {validate: val => /^(\w-*\.*)+@(\w-?)+(\.\w{2,})+$/.test(val), message: '邮箱格式有误'}],
      form: {
        username: '',
        password: '',
        phone: '',
        email: '',
        avatar: ''
      },
      labelPosition: 'top',
      loading: false,
      userInfo: {},
      isChangeImage: false,
      file: null
    }
  },
  created () {
    this.init()
  },
  methods: {
    init () {
      this.getUserInfo()
    },
    getUserInfo () {
      if (commonUtil.cookies('userInfo')) {
        this.userInfo = JSON.parse(commonUtil.cookies('userInfo'))
        this.form.username = this.userInfo.username
        this.form.phone = this.userInfo.phone
        this.form.email = this.userInfo.email
        this.form.avatar = this.userInfo.avatar ? 'http://localhost:3000/' + this.userInfo.avatar : defaultAvatar
      }
    },
    changeImage (files) {
      if (files.length > 0) {
        this.isChangeImage = true
        this.file = files[0]
        this.readFile(this.file)
      }
    },
    readFile (file) {
      var self = this
      var reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload = function (e) {
        self.form.avatar = this.result
      }
    },
    // 选择图片
    selectImg () {
      this.$refs.avatar.click()
    },
    async onUploadConfirm () {
      let formData = new FormData()
      formData.append('avatar', this.file)
      try {
        let res = await User.uploadImg(formData)
        if (res.code === 0 && res.data) {
          this.form.avatar = 'http://localhost:3000/' + res.data.avatar
          commonUtil.cookies('userInfo', JSON.stringify(res.data))
          this.$toast.success('上传成功!')
        }
      } catch (err) {
        console.log(err)
      }
    },
    onUploadCancel () {
      this.isChangeImage = false
    },
    async submit () {
      let opts = {
        ...this.form
      }
      try {
        let res = await User.modifyUserInfo(opts)
        if (res.code === 0) {
          this.updateUserInfo(res)
          this.$toast.success(res.data)
        } else {
          this.$toast.error('修改用户信息成功')
        }
      } catch (err) {
        this.$toast.error('修改用户信息失败')
      }
    },
    updateUserInfo (res) {
      this.userInfo = res.data
      commonUtil.cookies('userInfo', JSON.stringify(res.data))
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/css/variable.scss';
.information-title {
  text-align: left;
  margin-left: 50px;
}
.information-box {
  margin: 50px;
  max-width: 450px;
}

.information-form {
}

.input-group {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  &:nth-child(n + 2) {
    margin-top: 20px;
  }
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 100%;
}

.input-text {
  text-align: left;
  min-width: 60px;
  color: $form-text;
}

.input-value {
  text-align: left;
  display: flex;
  justify-content: flex-start;
  width: 100%;
  padding: 10px;
  outline: none;
  // border: 1px solid $information-input-border;
  // box-shadow: 0 0 10px $information-input-shadow;
  border: 1px solid $input-border;
  box-shadow: 0 0 10px $input-shadow;
}

.upload-btn {
  margin-left: 20px;
}
</style>
