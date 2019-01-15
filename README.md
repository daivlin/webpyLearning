# webpyLearning
在webpy学习中的一些总结

## 富文本编辑器
### CKeditor
- 优点
    1. 样式好看
    2. 功能强大
- 缺点
    1. 笨重，需要加载的内容多
    2. 配置复杂。
    
### nicEdit
- 定置内容：
    1. 原html的css中，ul的list-style被设置为none（bootstrap），所以将内容设置为有序列表与无序列表不能使用。
    2. 在nicEdit.js中英文标注改为中文。1046行，增加部分中文字体。如“宋体”、“黑体”
    3. 上传文件
        a. 配置处理上传文件的url。 uploadURI : '/test/uploadImg',
        b. 在处理url的后台程序，使用 POST处理，图片内容使用i=web.input(image={})获取。（浏览器的开发者工具可以看到request的表单内容）
        c. 返回json数据。数据包含“data”，1423行。还需link，图片path，1451行。如果需要默认图片宽度，还需返回width值。如下：
            
            import json
            web.header("Content-type:application/json;charset=utf-8")
            imgPath = "your image path"
            return json.dumps({"data":{"link":imgPath,"width":768}})
            
    4. 在文本框中显示：
        a. 返回的json数据中，获取图片路径link，添加倒文本框im中，1451行。
    5. 配置可以编辑html，方便调整图片宽度等。272行，在buttonList中，加入'xhtml'。
- 使用实例
---
    <!doctype html>
    <html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <script type="text/javascript" src="/static/nicEdit/nicEdit.js"></script>
        <title>test</title>
        <script type="text/javascript">
            bkLib.onDomLoaded(function() {
                new nicEditor({
                    fullPanel : false,
                    iconsPath : '/static/nicEdit/nicEditorIcons.gif',
                    uploadURI : '/test/uploadImg'
                    }).panelInstance('area1');
            });
        </script>    
    </head>
    <body>
        <div class="content">
            {% if formData %}
            {{ formData|safe }}
            {% endif %}
            <form method="post">
                <textarea rows="20" name="formData" id="area1" style="width:100%">{{ formData|safe }}</textarea>
                <input class="btn" type="submit" value="提交" />
            </form>
        </div>
    </body>
    </html>
---

## 子应用
1. 子应用中，web.ctx.fullpath是剥离了主应用的path，如果设置url（如：分页url，判断登陆转到主页）时，需手动设置：加上子应用前缀，或将web.ctx.fullpath改为web.ctx.homedomain。
2. web.seeother与web.ctx有关，注意路径问题。

