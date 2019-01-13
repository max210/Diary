- 校验与过滤
```
手机号：(/^(\+?0?86-?)?1[3456789]\d{9}$/).test(phone)
```
```
验证码：(/^\d{4}$/).test(code)
```
```
邮箱：(/^([0-9A-Za-z.\-_]+)@([0-9a-z]+\.[a-z]{2,3}(\.[a-z]{2})?)$/).test(email)
```
```
身份证号码：/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/
```
```
金额两位小数：money.replace(/[^\d.]/g, '').replace(/^\./g, '').replace(/\.{2,}/g, '.').replace('.', '$#$').replace(/\./g, '').replace('$#$', '.').replace(/^(-)*(\d+)\.(\d\d).*$/, '$1$2.$3')
```
```
过滤表情：xxx.replace(/(?:[\u2700-\u27bf]|(?:\ud83c[\udde6-\uddff]){2}|[\ud800-\udbff][\udc00-\udfff]|[\u0023-\u0039]\ufe0f?\u20e3|\u3299|\u3297|\u303d|\u3030|\u24c2|\ud83c[\udd70-\udd71]|\ud83c[\udd7e-\udd7f]|\ud83c\udd8e|\ud83c[\udd91-\udd9a]|\ud83c[\udde6-\uddff]|\ud83c[\ude01-\ude02]|\ud83c\ude1a|\ud83c\ude2f|\ud83c[\ude32-\ude3a]|\ud83c[\ude50-\ude51]|\u203c|\u2049|[\u25aa-\u25ab]|\u25b6|\u25c0|[\u25fb-\u25fe]|\u00a9|\u00ae|\u2122|\u2139|\ud83c\udc04|[\u2600-\u26FF]|\u2b05|\u2b06|\u2b07|\u2b1b|\u2b1c|\u2b50|\u2b55|\u231a|\u231b|\u2328|\u23cf|[\u23e9-\u23f3]|[\u23f8-\u23fa]|\ud83c\udccf|\u2934|\u2935|[\u2190-\u21ff])/g, '')
```

```
中英文字符长度：xxx.replace(/[^\x00-\xff]/g, '**').length
```

- 获取滚动距离兼容写法
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
function dealWith(data) {
  let result = []
  for (let i = 0; i < data.length; i++) {
    let obj = {time: '', monthList: []}
    let isNewItem = true
    data[i].monthTime = dayjs(data[i].createTime).format('yyyy年MM月)
    if (this.page === 1 && i === 0) {
      obj.time = data[i].monthTime
      obj.monthList.push(data[i])
      result.push(obj)
    } else {
      for (let j = 0; j < result.length; j++) {
        if (result[j].time === data[i].monthTime) {
          result[j].monthList.push(data[i])
          isNewItem = false
          break
        }
      }
      if (isNewItem) {
        obj.time = data[i].monthTime
        obj.monthList.push(data[i])
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
  let file = e.target.files[0]
  // this.$refs.cameraInput.value = '' // 兼容个别浏览器不会重复触发input的change事件

  let reader = new FileReader()
  reader.readAsDataURL(file)
  reader.onload = e => {
    const imgUnloadSrc = e.target.result // 缩略图地址
    let image = new Image()
    image.src = imgUnloadSrc
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

  let base64 = canvas.toDataURL(this.file.type || 'image/jpeg', 0.1) // image/jpeg 兼容部分安卓出现获取不到type的情况 压缩比例0.1

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
