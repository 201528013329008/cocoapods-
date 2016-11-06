# cocoapods-
cocoapods安装问题总结
## gem update失败

- This is a bug in the ruby gem installer of version 2.5.x. Patch the file installer.rb (on my machine in /usr/local/lib/ruby/2.3.0/rubygems/installer.rb) as follows:

Replace:
```ruby
if ruby_executable then
      question << existing
 ```
With:
```ruby
if ruby_executable then
      question << (existing || 'an unknown executable')
```



## pod setup失败
 
 
/usr/local/lib/ruby/site_ruby/2.3.0/rubygems.rb:270:in `find_spec_for_exe': can't find gem cocoapods (>= 0.a) (Gem::GemNotFoundException)
	from /usr/local/lib/ruby/site_ruby/2.3.0/rubygems.rb:298:in `activate_bin_path'
	from /usr/local/bin/pod:22:in `<main>'
	

 - 解决 
  - 自定义GEM_HOME
  - 
$ mkdir -p $HOME/Software/ruby
$ export GEM_HOME=$HOME/Software/ruby
$ gem install cocoapods
[...]
1 gem installed
$ export PATH=$PATH:$HOME/Sofware/ruby/bin
$ pod --version
0.37.2

## pod setup失败2
```
error: RPC failed; curl 56 SSLRead() return error -36

fatal: The remote end hung up unexpectedly

fatal: early EOF

fatal: index-pack failed
```
- 解决
 - git config --global http.postBuffer 24288000

 - pod setup或git clone https://github.com/CocoaPods/Specs.git master
 
 漫长等待
 
 安装好之后发现pod install还是太慢
 不仅要换gem源还要换repo源
 
  ``` 
pod repo remove master
pod repo add master https://coding.net/u/hging/p/Specs/git
pod repo update
```
###第二部还是出错RPC

 - 解决

 ``` 

  First, turn off compression:
git config --global core.compression 0
Next, let's do a partial clone to truncate the amount of info coming down:
git clone --depth 1 https://coding.net/u/hging/p/Specs/git
When that works, go into the new directory and retrieve the rest of the clone:
git fetch --unshallow 
Now, do a regular pull:
git pull --all
```

 ###第三部出错：
 - 
如果 pod setup还是自动创建master那么将~/.cocoapods/repos里面的那个文件夹的文字改成master

 - 记得将podfile文件加上source 'https://coding.net/u/hging/p/Specs/git'
