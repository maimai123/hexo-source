---
title: React-Quill插入视频bug 以及 图片上传oss
date: 2021-05-13
tags: react
---
hello~ 我好像真的好久好久没写博客了，对不起父老乡亲们，人越来越懒了
最近项目里用到了react-quill，这个编辑器，是react对quill的封装
但是发现了在插入视频的时候 插入的元素不是video标签，而是一个iframe
且每插入一遍后悔下载一次想要插入的视频，就去找了下解决办法

# let's start!
<font size=1>**如有转载，请注明出处。**</font>

> 正常用 React-Quill

```javascript
  import ReactQuill from 'react-quill';

  const [text, setText] = useState('');

  // 配置toolbar
  const toolbarContainer = [
    [{ size: ['small', false, 'large', 'huge'] }], // custom dropdown
    [{ font: [] }],
    [{ header: 1 }, { header: 2 }], // custom button values
    [{ header: [1, 2, 3, 4, 5, 6, false] }],
    ['bold', 'italic', 'underline', 'strike'], // toggled buttons
    [{ align: [] }],
    [{ color: [] }, { background: [] }],
    [{ indent: '-1' }, { indent: '+1' }], // outdent/indent
    [{ direction: 'rtl' }], // text direction
    ['blockquote', 'code-block'],

    [{ list: 'ordered' }, { list: 'bullet' }],
    ['image', 'video', 'link'],

    ['clean'],
  ];

  // 重新上传图片
  const modules = {
    toolbar: {
      container: toolbarContainer,
      handlers: {
        image: showImageUI,
      },
    },
  };

  const showImageUI = () => {
    const quillEditor = quillRef.current.getEditor();
    const input = document.createElement('input');
    input.setAttribute('type', 'file');
    input.setAttribute('accept', 'image/*');
    input.click();
    input.onchange = async () => {
      try {
        // 获取 signature
        const res = await fetchSignature();
        if (!res.success) return;
        const oss = res.data;
        const file = input.files[0];
        let formData = new FormData();
        const fileName = file.name;
        formData.append('key', `website/${oss['dir']}/${fileName}`);
        formData.append('dir', oss['dir']);
        formData.append('policy', oss['policy']);
        formData.append('OSSAccessKeyId', oss['accessid']);
        formData.append('success_action_status', '200');
        formData.append('signature', oss['signature']);
        formData.append('file', file, fileName);
        const host = oss.host.replace('http', 'https');
        // 上传 oss
        await axios.post(host, formData);
        const url = `${host}/website/${oss['dir']}/${fileName}`;
        const range = quillEditor.getSelection();
        quillEditor.insertEmbed(range.index, 'image', url);
      } catch (err) {
        console.error(err);
      }
    };
  };

  // 为啥这边要价格setTimeout呢？因为不加的话会有抖动效果
  const handleTextChange = (textTyped: any) => {
    setTimeout(() => {
      setText(textTyped);
    }, 10);
  };

  <ReactQuill
    ref={quillRef}
    theme="snow"
    modules={modules}
    value={text}
    onChange={() => handleTextChange}
  />

```
> 修改 video 按钮

```javascript
  // 引入 Quill
  import ReactQuill, { Quill } from 'react-quill';

  // 放在组件前
  const Video = Quill.import('formats/video');
  const Link = Quill.import('formats/link');

  class CoustomVideo extends Video {
    static create(value) {
      const node = super.create(value);

      const video = document.createElement('video');
      video.setAttribute('controls', true);
      video.setAttribute('type', 'video/mp4');
      video.setAttribute('style', 'width: 100%');
      video.setAttribute('src', this.sanitize(value));
      node.appendChild(video);

      return node;
    }

    static sanitize(url) {
      return Link.sanitize(url);
    }
  }
  CoustomVideo.blotName = 'video';
  CoustomVideo.className = 'ql-video';
  CoustomVideo.tagName = 'DIV';

  Quill.register('formats/video', CoustomVideo);
```

顺带说一下，用到富文本内容展示的时候 需要引入他的css
```javascript
import 'react-quill/dist/quill.snow.css';
```

More info: [麦麦](https://github.com/maimai123)
