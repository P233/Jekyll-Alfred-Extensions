Jekyll Alfred 2 Workflows
========================

Alfred 2 将之前的 extension 功能替换成了现在的 workflow，所以将这三个 Jekyll 相关插件简单调整了一下，重新发出来，欢迎大家试用。

### Create new Jekyll Post

输入 `newpost [title]`创建一篇 Jekyll 文章，文件名前自动添加今天的日期作为前缀，`title` 中请输入 `-` 代替空格。需要替换成自己的 Jekyll 文件夹的路径，格式必须以 `/Users/user_name/` 开头。

```
# Please replace your own _posts folder path in the next line. The format should start with /Users/user_name/
Dir.chdir "/Users/lpw/Dropbox/Websites/Jekyll/_posts/"

today = Time.now.strftime('%Y-%m-%d')
input = “{query}”

filename = [today, input].join('-')
extension = 'md'
	
file = [filename, extension].join('.')

system(%[touch "#{file}"])
system(%[open "#{file}"])
```


### Generate Jekyll site

输入 `jekyll` 运行 `cd [path]` 与 `jekyll --server --auto` 两个命令，然后在浏览器中访问网站，需要在 applescript 中替换自己的 Jekyll 文件夹路径。

```
on alfred_script(q)
tell application "Terminal"
    activate
    if (count of windows) is 0 then
        set currentTab to do script "cd ~/dropbox/websites/jekyll/"
    else
        do script "cd ~/dropbox/websites/jekyll/" in window 1
    end if
    do script "jekyll --server --auto" in window 1
    delay 2
    do shell script "open http://0.0.0.0:4000"
end tell
end alfred_script
```

### Push Jekyll to Github

输入 `pushjekyll` 将 Jekyll 生成的网站复制到新地址，并将新地址上传到 Github，之所以这样做是因为 Github Pages 不支持自定义插件，只好上传生成的静态网站。如果不需要这个步骤，可以将 `if` 部分删除。同样需要在 applescript 中替换自己的 Jekyll 文件夹路径，如果路径中有空格，需要在空格前添加 `\\`。

```
on alfred_script(message)
  tell application "Terminal"
    activate

    if (count of windows) is 0 then
        set currentTab to do script "cp -a ~/dropbox/websites/jekyll/_site/. ~/dropbox/websites/p233.github.com/"
    else
        do script "cp -a ~/dropbox/websites/jekyll/_site/. ~/dropbox/websites/p233.github.com/" in window 1
    end if
    
    do script "cd ~/dropbox/websites/p233.github.com/" in window 1
    do script "git add ." in window 1
    do script "git commit -am '" & message & "'" in window 1
    do script "git push" in window 1
  end tell
end alfred_script
```
