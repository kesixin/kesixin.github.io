上一篇文章已经编写了PC端的裁剪图片案例，这次是涉及移动端的头像裁剪更换，类似于微信更换头像功能。。。

**案例下载：**<a href="http://download.csdn.net/download/kesixin/9951656"  target="_blank">http://download.csdn.net/download/kesixin/9951656</a>

思路跟PC端的案例是一样的，这里就不多说了。

将案例放到服务器上，如果出现上传失败可能是：
1.提交处理服务url出错：main.js
```
$(document.body).on('click', '[data-method]', function () {
      var data = $(this).data(),
          $target,
          result;

      if (data.method) {
        data = $.extend({}, data); // Clone a new one

        if (typeof data.target !== 'undefined') {
          $target = $(data.target);

          if (typeof data.option === 'undefined') {
            try {
              data.option = JSON.parse($target.val());
            } catch (e) {
              console.log(e.message);
            }
          }
        }

        result = $image.cropper(data.method, data.option);

        if (data.method === 'getCroppedCanvas') {
//          $('#getCroppedCanvasModal').modal().find('.modal-body').html(result);
//          alert(result.toDataURL('image/jpeg'));
//          $.post('/index.php/sdasdf',result.toDataURL('image/jpeg'),function(){},'json');
            var imgBase=result.toDataURL('image/jpeg');
            if(imgBase!=""){
                var data = {imgBase: imgBase};
                //提交地址
                $.post('../mobile/upload.php', data, function (ret) {
                    if(ret=='true'){
		                alert('上传成功');
		              }else{
		                alert('上传失败');
		              }
                }, 'text');
            }
        }
        
        if ($.isPlainObject(result) && $target) {
          try {
            $target.val(JSON.stringify(result));
          } catch (e) {
            console.log(e.message);
          }
        }

      }
    }).on('keydown', function (e) {

      switch (e.which) {
        case 37:
          e.preventDefault();
          $image.cropper('move', -1, 0);
          break;

        case 38:
          e.preventDefault();
          $image.cropper('move', 0, -1);
          break;

        case 39:
          e.preventDefault();
          $image.cropper('move', 1, 0);
          break;

        case 40:
          e.preventDefault();
          $image.cropper('move', 0, 1);
          break;
      }

    });
```

实现效果：

![](http://upload-images.jianshu.io/upload_images/6673460-5ca3cd861ddadaec.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
