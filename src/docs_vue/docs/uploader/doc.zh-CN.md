# Uploader 上传组件

### 介绍

提供浏览器原生上传功能

### 安装使用

```tsx
import "quarkd/lib/uploader";
```

### 基础用法

```html
<quark-uploader :preview="false" @afterread="afterRead"></quark-uploader>
```

```js
function afterread(file) {
  console.log(file.file.name);
}
```

### 文件预览

默认开启预览功能

```html
<quark-uploader preview ref="uploadStatus"></quark-uploader>
```

```js
mounted() {
  this.$refs.preview.
  setPreview(['https://img.yzcdn.cn/vant/leaf.jpg', 'https://img.yzcdn.cn/vant/leaf.jpg');
}
```

### 上传状态

默认三种状态 uploading、 done、failed，其中 failed 会移除当前失败文件, 默认为 done

```html
<quark-uploader preview ref="preview" @afterread="afterReadStatus"
  ><quark-uploader></quark-uploader
></quark-uploader>
```

```js
mounted() {
  this.$refs.uploadStatus.
  setPreview(['https://img.yzcdn.cn/vant/leaf.jpg']);
},
methods: {
    // 暂时只做了单独上传示例，多选上传方法也一样
    async afterReadStatus({ detail: file }) {
      cosnt { uploadStatus }= this.$refs
      uploadStatus.value.setStatus({
        ...file,
        status: "uploading",
        message: "上传中",
      });
      await sleep(2000);
      uploadStatus.value.setStatus({
        ...file,
        status: "failed",
      });
      Toast.success(`${file.file.name}上传失败`);
  };
}
```

### 限制上传数量

超过数量，隐藏多余部分并且隐藏上传按钮

```html
<quark-uploader maxcount="2" preview></quark-uploader>
```

### 限制上传大小

maxsize 单位 B 起步，例如 1M 是 1024 \* 1024

```html
<quark-uploader maxsize="1024" @oversize="oversize"></quark-uploader>
```

```js
oversize() {
  console.log('有文件超过1KB了哦')
}
```

### 自定义上传样式

```html
<quark-uploader preview>
  <div slot="uploader">上传文件</div>
</quark-uploader>
```

### 自定义删除图片 icon

使用 closeimg 属性，接收一个图片地址字符串, 并能通过 css 变量改变图片大小位置

```html
<div class="flex closeimg">
  <quark-uploader
    preview
    ref="customPreviewIcon"
    closeimg="https://m.hellobike.com/resource/helloyun/15697/dEYF0_round_close_fill.png?x-oss-process=image/quality,q_80"
  >
  </quark-uploader>
</div>
```

```css
.closeimg {
  --uploader-delete-top: 0px;
  --uploader-delete-right: 0px;
  --uploader-delete-wrap-width: 14px;
  --uploader-delete-wrap-height: 14px;
  --uploader-delete-background: transparent;
  --uploader-delete-left-radius: 12px;
}
```

### 上传前置

beforeUpload 返回 Boolean, false 阻止上传。

```js
mounted() {
  this.$refs.before.beforeUpload = this.beforeUpload
},
beforeUpload(files) {
  const r = files.every(file => file.type === 'image/jpg');
  if(!r) {
    console.log('请上传 jpg 格式图片');
    return false
  };
  return true;
},
```

### 禁止上传

```html
<quark-uploader preview disabled></quark-uploader>
```

### 只读预览模式

```html
<quark-uploader preview readonly />
```

## API

### Props

| 参数       | 说明                                              | 类型                                       | 默认值             |
| ---------- | ------------------------------------------------- | ------------------------------------------ | ------------------ |
| accept     | 允许上传的文件类型                                | `string`                                   | `image/*`          |
| multiple   | 是否多选                                          | `boolean`                                  | `true`             |
| preview    | 是否展示预览                                      | `boolean`                                  | `false`            |
| capture    | 图片选取模式，可选值为 `boolean` (直接调起摄像头) | `false`                                    |
| maxcount   | 最大上传数量，超出隐藏                            | `string`                                   |
| maxsize    | 最大上传大小                                      | `string`                                   | `26214400 （25M）` |
| disabled   | 禁止上传                                          | `boolean`                                  | `false`            |
| hidedelete | 隐藏删除图标                                      | `boolean`                                  | `false`            |
| closeimg   |                                                   |                                            |
| readonly   | 只读模式                                          | `boolean`                                  | `false`            |
| afterread  | 上传后回调                                        | `(file: file or file[]) => void`           |                    |
| oversize   | 配合 maxsize 使用，超过大小回调函数               | `(items: fiel[], maxsize: string) => void` |                    |
| onRemove   | 需配合 beforeDelete 不可单独使用                  | `(items: file[]) => void`                  |                    |

### slot

| 名称        | 说明           |
| ----------- | -------------- | --- |
| name=loader | 自定义上传组件 |     |

### Methods

| 名称         | 说明                                                                | 类型                              |
| ------------ | ------------------------------------------------------------------- | --------------------------------- |
| beforeUpload | 上传前置，用法 uploader.beforeUpload = fn                           | `(fn: () => boolean) => void`     |
| setPreview   | 初始化预览数据，用法 uploader.setPreview(data)                      | `(url: string[]) => void`         |
| beforeDelete | 删除前置, 需配合 onRemove 一起使用，用法 uploader.beforeDelete = fn | `(file, {index: number}) => void` |
| closePreview | 手动关闭预览弹窗方法                                                |                                   |
| setStatus    | 上传期间设置上传状态 uploader.setStatus(file)                       | fn(file)                          |

## 样式变量

组件提供了以下[CSS 变量](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties)，可用于自定义样式，使用方法请参考[主题定制](#/zh-CN/guide/theme)。

| 名称                            | 说明                   | 默认值         |
| ------------------------------- | ---------------------- | -------------- |
| `--uploader-width`              | 上传、预览组件的宽度   | `93px`         |
| `--uploader-height`             | 上传、预览组件的高度   | `93px`         |
| `--uploader-border-width`       | 上传组件的边框         | `1px`          |
| `--uploader-border-color`       | 上传组件的边框色       | `#E1E6EB`      |
| `--uploader-border-style`       | 上传组件的风格         | `dashed`       |
| `--uploader-radius`             | 上传、预览组件的边框   | `4px`          |
| `--uploader-margin`             | 上传、预览组件右下间距 | `6px`          |
| `--uploader-delete-wrap-width`  | 预览删除按钮宽度       | `14px`         |
| `--uploader-delete-wrap-height` | 预览删除按钮高度       | `14px`         |
| `--uploader-delete-background`  | 删除组件的背景色       | `rgb(0, 0, 0)` |
| `--uploader-delete-color`       | 删除组件图标颜色       | `#fff`         |
| `--uploader-delete-size`        | 删除组件 图标大小      | `10px`         |
| `--uploader-delete-top`         | 删除组件图标向上位置   |
| `--uploader-delete-right        | 删除组件图标向上位置   |
| `--uploader-delete-wrap-width   | 删除组件图标宽度       |
| `--uploader-delete-wrap-heigh   | 删除组件图标高度       |
| `--uploader-delete-background   | 删除组件图标背景色     |
| `--uploader-delete-left-radius  | 删除组件图标圆角       |
