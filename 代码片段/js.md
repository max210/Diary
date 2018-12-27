- 校验
```
手机号：(/^(\+?0?86-?)?1[3456789]\d{9}$/).test(phone)
```
```
手机号：(/^\d{4}$/).test(code)
```
```
邮箱：(/^([0-9A-Za-z.\-_]+)@([0-9a-z]+\.[a-z]{2,3}(\.[a-z]{2})?)$/).test(email)
```
```
金额两位小数：money.replace(/[^\d.]/g, '').replace(/^\./g, '').replace(/\.{2,}/g, '.').replace('.', '$#$').replace(/\./g, '').replace('$#$', '.').replace(/^(-)*(\d+)\.(\d\d).*$/, '$1$2.$3')
```
- 滚动特殊处理
```
const scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
```
- 网络变化监听
```
function listenLine () {
  const el = document.body
  if (el.addEventListener) {
    window.addEventListener('online', () => {}, true)
    window.addEventListener('offline', () => {}, true)
  } else if (el.attachEvent) {
    window.attachEvent('ononline', () => {})
    window.attachEvent('onoffline', () => {})
  } else {
    window.ononline = () => {}
    window.onoffline = () => {}
  }
}
```
- 生产根据日期分组的数据
```
function dealWith() {
  let result = []
  for (let i = 0; i < originData.length; i++) {
    let obj = {time: '', monthList: []}
    let isNewItem = true
    originData[i].monthTime = formatDate(new Date(originData[i].createTime), 'yyyy年MM月')
    originData[i].detailTime = formatDate(new Date(originData[i].createTime), 'yyyy年MM月dd日 hh:mm')
    if (this.page === 1 && i === 0) {
      obj.time = originData[i].monthTime
      obj.monthList.push(originData[i])
      result.push(obj)
    } else {
      for (let j = 0; j < result.length; j++) {
        if (result[j].time === originData[i].monthTime) {
          result[j].monthList.push(originData[i])
          isNewItem = false
          break
        }
      }
      if (isNewItem) {
        obj.time = originData[i].monthTime
        obj.monthList.push(originData[i])
        result.push(obj)
      }
    }
  }
  return  result
}
```
- 上传图片并压缩
```
inputChange (e) {
  if (typeof FileReader === 'undefined') {
    toast.error('您的浏览器不支持上传图片')
    return
  }
  toast.loading.show()
  this.file = e.target.files[0]
  this.$refs.cameraInput.value = '' // 兼容个别浏览器不会重复触发input的change事件
  console.log('this.file', this.file)

  let reader = new FileReader()
  reader.readAsDataURL(this.file)
  reader.onload = e => {
    this.imgUnloadSrc = e.target.result
    let image = new Image()
    image.src = e.target.result
    image.onload = () => {
      const maxSize = 500 * 1024 // 500KB
      if (this.file.size > maxSize) {
        this.compressImg(image)
      } else {
        this.uploadImg(this.file)
      }
    }
  }
},
// 压缩图片并返回文件对象
compressImg (image) {
  let canvas = document.createElement('canvas')
  let ctx = canvas.getContext('2d')
  canvas.width = image.width
  canvas.height = image.height
  ctx.drawImage(image, 0, 0, canvas.width, canvas.height)

  let base64 = canvas.toDataURL(this.file.type || 'image/jpeg', 0.1) // image/jpeg 兼容部分安卓出现获取不到type的情况

  // base64转为blob
  let binaryString = window.atob(base64.split(',')[1])
  let arrayBuffer = new ArrayBuffer(binaryString.length)
  let intArray = new Uint8Array(arrayBuffer)

  for (let i = 0, j = binaryString.length; i < j; i++) {
    intArray[i] = binaryString.charCodeAt(i)
  }

  let blob

  try {
    blob = new Blob([intArray], { type: this.file.type })
  } catch (error) {
    let BlobBuilder = window.BlobBuilder || window.WebKitBlobBuilder || window.MozBlobBuilder || window.MSBlobBuilder
    if (error.name === 'TypeError' && window.BlobBuilder) {
      let builder = new BlobBuilder()
      builder.append(arrayBuffer)
      blob = builder.getBlob()
    } else {
      toast.error('版本过低，不支持上传图片')
      return
    }
  }
  const fileOfBlob = new File([blob], this.file.name)
  console.log('fileOfBlob', fileOfBlob)
  this.uploadImg(fileOfBlob)
},
// 上传图片
uploadImg (file) {
  let formData = new FormData()
  formData.append('type', this.file.type)
  formData.append('size', file.size)
  formData.append('name', this.file.name)
  formData.append('lastModifiedDate', this.file.lastModifiedDate)
  formData.append('file', file)
  console.log('formdata', formData)
}
```
- 滑动组件 vue-awesome-swiper
```
// 滑动组件配置
swiperOption: {
  loop : true,
  autoplay: {
    disableOnInteraction: false
  },
  slidesPerView: 'auto',
  centeredSlides: true,
  spaceBetween: 30,
  pagination: {
    el: '.swiper-pagination',
    bulletActiveClass: 'my-bullet-active',
    clickable: true
  }
}
// 自定义css
.my-bullet-active {
  opacity: 1!important;
  background: #D7214A!important;
}
```
- 格式化时间
```
const formatDate = (date, fmt) => {
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
  }
  let o = {
    'M+': date.getMonth() + 1,
    'd+': date.getDate(),
    'h+': date.getHours(),
    'm+': date.getMinutes(),
    's+': date.getSeconds(),
    'S+': date.getMilliseconds()
  }
  for (let k in o) {
    if (new RegExp(`(${k})`).test(fmt)) {
      let str = o[k] + ''
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? str : ('00' + str).substr(str.length))
    }
  }
  return fmt
}

formatDate(Date.now(), 'yyyy年MM月dd日 hh:mm')
```
- axios 封装
```
// post方法改变为formdata形式
function transformRequest (data) {
  let ret = ''
  for (let it in data) {
    if ((typeof data[it]) === 'string') {
      ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
    } else {
      ret += encodeURIComponent(it) + '=' + encodeURIComponent(JSON.parse(data[it])) + '&'
    }
  }
  return ret
}

// 创建axios实例
const service = axios.create({
  baseURL: PREFIX, // api的base_url
  timeout: 15000, // 请求超时时间
  validateStatus: status => {
    return status >= 200 && status < 500 // 判断状态码 返回true则进入resolve
  }
})

// request拦截器
service.interceptors.request.use(config => {
  config.headers['Content-Type'] = 'application/json;charset=utf-8'

  return config
}, error => {
  if (navigator && !navigator.onLine) {
    toast.error('网络不可用')
  } else {
    toast.error('服务繁忙，请稍后重试')
  }
  console.log(error) // for debug
  return Promise.reject(error)
})

// respone拦截器
service.interceptors.response.use(
  response => {
    // token 失效
    if (response.data && +response.data.status === 20) {
      toast.error('请重新登录')
      setTimeout(() => {
        dsbridge.call('userLogout')
      }, 1000)
    }
    // 无token
    if (response && +response.status === 401) {
      toast.error('请重新登录')
      setTimeout(() => {
        dsbridge.call('userLogout')
      }, 1000)
      return Promise.resolve(response.status)
    }

    return Promise.resolve(response.data)
  },
  error => {
    console.log('catch Error >>> ', error, '\nError url >>>', error.config.url)
    if (navigator && !navigator.onLine) {
      toast.error('网络不可用')
    } else {
      toast.error('服务繁忙，请稍后重试')
    }

    return Promise.reject(error)
  }
)
```
- url 取参数
```
let loc = window.location

let urlQuery = {
  queryOne: function (name, str) {
    let s = ''
    if (str) {
      s = str
    } else {
      s = loc.search
    }
    let reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)') // 构造一个含有目标参数的正则表达式对象
    let r = s.substr(1).match(reg) // 匹配目标参数
    if (r != null) {
      return decodeURIComponent(r[2])
    }
    return null // 返回参数值
  },

  queryAll: function (str) {
    let s = ''
    if (str) {
      s = str
    } else {
      s = loc.search
    }
    let search = s.substr(1)
    let a = search.split('&')
    let i = 0
    let result = {}

    while (a[i]) {
      let kv = a[i].split('=')
      result[decodeURIComponent(kv[0])] = decodeURIComponent(kv[1])
      i++
    }
    return result
  }
}
```
- 判断移动 PC
```
function isPC () {
  if (/Android|webOS|iPhone|iPad|Windows Phone|SymbianOS|BlackBerry/i.test(navigator.userAgent)) {
    return false
  } else {
    return true
  }
}
```
