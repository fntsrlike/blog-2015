---
layout: post
title: "Sublime Text 2"
date: 2013-12-15 15:03:12 +0800
comments: true
categories: [it]
tags: [editor]
---

ST2 是我在 Programming 時必備的編輯器（Editor），從原本使用 Notepad++ 跳槽到 ST2，就回不去了，更遑論筆電換成 Mac 後，更是確定使用 ST2 作為我的開發利器（雖然也不是不會使用神器 Vim，但是很多快捷鍵還不熟，通常只有在連線到 server 才會用ＸＤ）。而在處理 MBA 2013 年中升級到 OS X 10.9的 續航問題而嘗試重新安裝 OS 後，便利用這機會，把我有關 ST2 的相關設定作為筆記寫成這篇文章囉。

## 為什麼使用ST2

### 我的理由：

- **跨平台：**Linux, OS X, Windows 皆有支援。
- **配色佳：**尤愛預設的Monokai Color Scheme，深底色會讓眼睛舒適。
- **套件多：**豐富的套件與簡單好用的套件管理器，讓我簡單擴充我想要的功能。
- **自訂強：**非常彈性與強大的自訂設定，讓我打造屬於自己的編輯器。

### ST設計原則：

- 專注在文字與程式碼上，而不是讓人眼花撩亂的工具列；
- 對話框訊息不使用晦澀難懂的文字；
- 善用螢幕的每個空間，使全螢幕、多螢幕模式一起編輯檔案，儘可能很容易

<!-- more -->

## 下載
下載頁面：[http://www.sublimetext.com/2](http://www.sublimetext.com/2)
因為 ST3 在此時還在 Beta，有些套件可能仍還未升級到可以支援 ST3，所以本人還是繼續使用 ST2。

## 套件（Packages）

### 安裝套件管理器
Ctrl + ` 呼叫 Console（控制台），然後輸入以下 Script（腳本），並且送出：

{% highlight python%}
import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print('Please restart Sublime Text to finish installation')
{% endhighlight %}

如此一來就可以使用套件管理器了！

### 使用套件管理器

使用command + shift + p 呼叫 Command Palette。然後你可以使用下列指令來做套件管理：

- Package Control: Install Package
- Package Control: Litst Package
- Package Control: Remove Package

Command Palette的搜尋是只要輸入目標擁有的關鍵字即可搜尋到，所以通常我們只輸入後方的單字。還有其他指令可輸入Package Control 查詢

以下套件名稱都是可以點擊的超連結，會連結該套件在Package Control的對應頁面。有些套件不是安裝就能使用，或是需要透過按鍵觸發，建議安裝前都閱讀一下他們自己的頁面哦。

### 推薦套件

- [Package Control](https://sublime.wbond.net/packages/Package%20Control) - 就是上面安裝的套件管理器。
- [AdvancedNewFile](https://sublime.wbond.net/packages/AdvancedNewFile) - 在指定路徑新增檔案。
- [Alignment](https://sublime.wbond.net/packages/Alignment) - 讓程式碼能多行將'='或自定義符號對齊的套件。
- [BracketHighlighter](https://sublime.wbond.net/packages/BracketHighlighter) - 將括號高亮顯示。
- [ConvertToUTF8](https://sublime.wbond.net/packages/ConvertToUTF8) - 解決中文顯示亂碼的問題（將Big5, GBK轉為UTF8讓編輯器顯示）
- [ColorHighlighter](https://sublime.wbond.net/packages/Color%20Highlighter) - 當的游標移至文字中如顏色相關的色碼會自動顯示對應的顏色。
- [ColorPicker](https://sublime.wbond.net/packages/ColorPicker) - 呼叫調色盤，讓你直接用選的來選顏色。
- [Emmet](https://sublime.wbond.net/packages/Emmet) - 原Zend Coding，能加速程式開發的神器，詳細使用方法請見官方文件。
- [LiveReload](https://sublime.wbond.net/search/LiveReload) - 此套件搭配對應瀏覽器擴充，可以讓你儲存檔案時，自動重新整理網頁。
- [SFTP](https://sublime.wbond.net/packages/SFTP) - 能夠讓你使用SFTP/FTP的方式，直接用本機的ST去修改檔案並且儲存。對於Vim苦手可說是必備套件！
- [SideBarEnhancements](https://sublime.wbond.net/packages/SideBarEnhancements) - Enhancements to Sublime Text sidebar. Files and folders.
- [SublimeLinter](https://sublime.wbond.net/packages/SublimeLinter) - 能用高亮提示使用者程式碼中，不是規範內或錯誤的寫法。
- [SublimeCodeIntel](https://sublime.wbond.net/packages/SublimeCodeIntel) - 多數語言的程式碼提示與追蹤。


### 選用套件

- [Console API Snippets (JavaScript)](https://sublime.wbond.net/packages/Console%20API%20Snippets%20(JavaScript)) - JavaScript Console API Snippets for Sublime Text
- [CSS Less(ish)](https://sublime.wbond.net/packages/CSS%20Less(ish)) - 讓你用註解的方式，在CSS達到LESS用變數、嵌套的功能。
- [DocBlockr](https://sublime.wbond.net/packages/DocBlockr) - 自動完成PHP, JS, CoffeeScript, ActionScript, C, C++的DocBlock註解。
- [Git](https://sublime.wbond.net/packages/Git) - 能在ST下使用Git指令，免於在ST和Terminal間頻繁地切換。
- [HTML5](https://sublime.wbond.net/packages/HTML5) - HTML5程式碼高亮與自動完成。
- [jQuery](https://sublime.wbond.net/packages/jQuery) - jQuery程式碼高亮與自動完成。
- [JsFormat](https://sublime.wbond.net/packages/JsFormat) - 幫你格式化JavaScript排版，尤其使用在壓縮過的js檔上。
- [LESS](https://sublime.wbond.net/packages/LESS) - LESS程式碼高亮
- [Prefixr](https://sublime.wbond.net/packages/Prefixr) - 能透過Prefixr API協助處理CSS跨瀏覽器的相容問題。
- [Sass](https://sublime.wbond.net/packages/Sass) - SASS程式碼高亮與自動完成。
- [SCSS](https://sublime.wbond.net/packages/SCSS) - SCSS程式碼高亮與自動完成。
- [Tag](https://sublime.wbond.net/packages/Tag) - 重新格式化HTML/XML的排版。適合用在外來的dirty code。


## 自定義設定檔
{% highlight json%}
{
    "font_size": 14.0,   // 字體大小
    "rulers": [120],     // 邊線寬度
    "wrap_width": 120,   // 邊界寬度
    "tab_size": 4,       // tab寬度
    "spell_check": true, // 拼音檢查
    "translate_tabs_to_spaces": true,          // tab轉成空白
    "trim_trailing_white_space_on_save": true, // 存檔時將句子後面多餘的空白清除
    "highlight_line": true,        // 高亮當前行
    "match_brackets_angle": true,  // 顯示對應的尖型括號 "<>"
    "save_on_focus_lost": true,        // 當不再專注當前文件時，自動存檔
    "ignored_packages":["Vintage"] // 忽略套件
}
{% endhighlight %}


## 參考資料
<span/>

- [Sublime Text 手冊](http://docs.sublimetext.tw/)
- [Package Control](https://sublime.wbond.net/)
- [KingKong Bruce記事 - 實用SUBLIME TEXT 2套件整理(2012/12)](http://blog.kkbruce.net/2012/12/useful-sublime-text-2-package-list.html#.Uq1ewGQW1B8)
- [梦想天空 - 借助 SublimeLinter 编写高质量的 JavaScript & CSS 代码](http://www.cnblogs.com/lhb25/archive/2013/05/02/sublimelinter-for-js-css-coding.html)
- [mrkt 的程式學習筆記 - Sublime Text 2 - 好用的前端程式編輯器 Part.5 使用Alignment對齊你的Code](http://kevintsengtw.blogspot.tw/2012/03/sublime-text-2-part5-alignmentcode.html#.Uq1lu2QW1B8)
- [ihower {blogging} - Sublime Text 資源整理](http://ihower.tw/blog/archives/7375)
- [{euSpark*} U閃亮 - \[開發筆記\] 幫你的 Sublime Text 2安裝 LiveReload Plug-in](http://eugg.blogspot.tw/2013/05/sublime-text-2-livereload-plug-in.html)
- [Sublime Text初始設置](http://rritw.com/a/JAVAbiancheng/ANT/20120910/220800.html)
