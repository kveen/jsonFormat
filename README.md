# jsonFormat

    // Checkbix.init();
    function format(txt, compress/*是否为压缩模式*/) {/* 格式化JSON源码(对象转换为JSON文本) */
        var indentChar = '    ';
        if (/^\s*$/.test(txt)) {
            var options = {
                title: "调用服务",
                message: "数据为空,无法格式化!"
            };
            bootbox.alert(options);
            return;
        }
        try {
            var data = eval('(' + txt + ')');
        }
        catch (e) {
            var options = {
                title: "调用服务",
                message: '数据源语法错误,格式化失败!'
            };
            bootbox.alert(options);
            return;
        }
        ;
        var draw = [], last = false, This = this, line = compress ? '' : '\n', nodeCount = 0, maxDepth = 0;

        var notify = function (name, value, isLast, indent/*缩进*/, formObj) {
            nodeCount++;
            /*节点计数*/
            for (var i = 0, tab = ''; i < indent; i++)tab += indentChar;
            /* 缩进HTML */
            tab = compress ? '' : tab;
            /*压缩模式忽略缩进*/
            maxDepth = ++indent;
            /*缩进递增并记录*/
            if (value && value.constructor == Array) {/*处理数组*/
                draw.push(tab + (formObj ? ('"' + name + '":') : '') + '[' + line);
                /*缩进'[' 然后换行*/
                for (var i = 0; i < value.length; i++)
                    notify(i, value[i], i == value.length - 1, indent, false);
                draw.push(tab + ']' + (isLast ? line : (',' + line)));
                /*缩进']'换行,若非尾元素则添加逗号*/
            } else if (value && typeof value == 'object') {/*处理对象*/
                draw.push(tab + (formObj ? ('"' + name + '":') : '') + '{' + line);
                /*缩进'{' 然后换行*/
                var len = 0, i = 0;
                for (var key in value)len++;
                for (var key in value)notify(key, value[key], ++i == len, indent, true);
                draw.push(tab + '}' + (isLast ? line : (',' + line)));
                /*缩进'}'换行,若非尾元素则添加逗号*/
            } else {
                if (typeof value == 'string')value = '"' + value + '"';
                draw.push(tab + (formObj ? ('"' + name + '":') : '') + value + (isLast ? '' : ',') + line);
            }
            ;
        };
        var isLast = true, indent = 0;
        notify('', data, isLast, indent, false);
        return draw.join('');
    }

    function isJsonFormat(str) {
        try {
            JSON.parse(str);
        } catch (e) {
            return false;
        }
        return true;
    }

// 添加
    function col_add(dom, val, text) {
        var obj = '#' + dom
        var selObj = $(obj);
        var value = "value";
        var text = text;
        var str = "<option value='" + val + "'>" + text + "</option>"
        selObj.append(str);
    }

// 清空
    function col_clear(dom) {
        var obj = '#' + dom + ' option'
        var selOpt = $(obj);
        selOpt.remove();
    }

    function showInfo(data) {
        try {

            // $('#commandInfo').val(data);
            $('#jsonString').val(data);
            var options = {
                dom: '#jsonContent' //对应容器的css选择器
            };
            var jf = new JsonFormater(options); //创建对象
            jf.doFormat($('#jsonString').val()); //格式化json
        } catch (e) {
            var showInfoObj = {"data": "data is error"};

            // $('#commandInfo').val(showInfoObj);
            $('#jsonString').val(showInfoObj);
            var options = {
                dom: '#jsonContent' //对应容器的css选择器
            };
            var jf = new JsonFormater(options); //创建对象
            jf.doFormat($('#jsonString').val()); //格式化json
        }
    }
