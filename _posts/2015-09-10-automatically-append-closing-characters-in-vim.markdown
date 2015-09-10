---
layout: post
title: "Vim中的自动补全"
date: "2015-09-10 22:48"
---

### 方案一
先贴代码了,你可以将下面代码直接复制到你的`.vimrc`文件中，下次重启vim自动加载后就会生效了。

{% highlight vim linenos %}
inoremap ( ()<LEFT>
inoremap [ []<LEFT>
inoremap { {}<LEFT>
inoremap " ""<LEFT>
inoremap ' ''<LEFT>
inoremap < <><LEFT>

function! RemovePairs()
    let s:line = getline(".")
    let s:previous_char = s:line[col(".")-1]

    if index(["(","[","{"],s:previous_char) != -1
        let l:original_pos = getpos(".")
        execute "normal %"
        let l:new_pos = getpos(".")
        " only right (
        if l:original_pos == l:new_pos
            execute "normal! a\<BS>"
            return
        end

        let l:line2 = getline(".")
        if len(l:line2) == col(".")
            execute "normal! v%xa"
        else
            execute "normal! v%xi"
        end
    else
        execute "normal! a\<BS>"
    end
endfunction

function! RemoveNextDoubleChar(char)
    let l:line = getline(".")
    let l:next_char = l:line[col(".")]

    if a:char == l:next_char
        execute "normal! l"
    else
        execute "normal! i" . a:char . ""
    end
endfunction

inoremap <BS> <ESC>:call RemovePairs()<CR>a
inoremap ) <ESC>:call RemoveNextDoubleChar(')')<CR>a
inoremap ] <ESC>:call RemoveNextDoubleChar(']')<CR>a
inoremap } <ESC>:call RemoveNextDoubleChar('}')<CR>a
inoremap > <ESC>:call RemoveNextDoubleChar('>')<CR>a
{% endhighlight %}

上面代码，分别自定义了insert模式下的`{、[、（、<、”、‘`自动补全键位映射,之后定义了两个函数，分别用来处理添加自动补全后带来的两个问题:

1. 删除问题(即之前直接删除右括号就可以，现在还需要后移删除左括号)
2. 自动补全后人为习惯性的成对输入括号。

第一个问题，通过`%`来查看括号成对情况，根据括号出现的位置分成三种情况处理。

第二个问题的解决方案是，判断用户输入是否与下一个紧挨着的字符相同，相同则忽略这次输入，否则照常输入即可。

因为双引号、单引号以及大于、小于不支持通过`%`来查找成对括号，所以也不适用上面两个算法，不过这四个字符上面两个问题对其影响并不明显。

最后分别把这两个函数与删除回退(delete)键位，以及`}、]、>、）`进行映射。

在v2ex看到问怎么退出补全后的括号，解决方法直接`<ESC>la`即可。你也可以把它写到映射中，通过F1~F12这样的功能键进行一键处理，个人倾向于肌肉记忆。

{% highlight vim linenos %}
inoremap <F9> <ESC>la
{% endhighlight %}


####方案二
这个方案是直接用`Snipmate`这样的插件,定义括号补全的代码片段，通过`tab`键操作
{% highlight vim linenos %}
snippet [
    [${1}]${2}
{% endhighlight %}
上面是方括号的，其它括号类似. 这样做确实比较轻量，直接输入括号也不影响，适合有使用Snipmate的同学.


### 参考引用
+ [http://oldj.net/article/vim-parenthesis/](http://oldj.net/article/vim-parenthesis/)
+ [http://learnvimscriptthehardway.stevelosh.com/](http://learnvimscriptthehardway.stevelosh.com/)
