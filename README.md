Jekyll-Alfred-Extensions
========================

Three Alfred Extensions: Create a Jekyll Post, Generate Jekyll site, and Push Jekyll post to Github. You may download this repo or build your own extension with the following code. Please don't forget to change the paths.

## Create a Jekyll Post

```
# Please replace your own _posts folder path in the next line. The format should start with /Users/user_name/
Dir.chdir "/Users/lpw/Dropbox/Websites/Jekyll/_posts/"

today = Time.now.strftime('%Y-%m-%d')
input = ARGV[0]

filename = [today, input].join('-')
extension = 'md'
	
file = [filename, extension].join('.')

system(%[touch "#{file}"])
system(%[open "#{file}"])
```

## Generate Jekyll site

```
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
```

## Push Jekyll post to Github

I prefer to upload the generated site as Github Pages doesn't support custom plugins. If you don't need this step, just remove the `if` syntax.

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