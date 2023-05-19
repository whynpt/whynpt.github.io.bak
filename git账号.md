# windows下git和vscode使用
我认为合格的开发者都应该在linux下工作，本人在读研和前司工作期间就一直在ubuntu下写代码。但现在懒的在台式机上安装ubuntu，且想试试win下开发，所以这里记录win下搭建开发环境。git安装没有需要特别留意的地方，安装过程中，可以自动选择加入到环境变量。这里记录如何完成账号设置，是开发流程尽量接近前司。  
github创建账号无需赘述，我在秋招期间已经注册好账号，并且把数据结构的代码上传到github。那会我使用的是VS2019，好像是完全在IDE里完成版本控制，没有使用git指令。前司的开发流程大体是这样的：  
1.在IDE中完成源代码编辑  
2.git status/add/comit/push提交代码至gerrit  
3.git clone/git cherrypick下载库或提交，或者git pull/repo sync更新本地代码  
4.git push提交代码  
为了在我台式机上也能实现上述的开发流程，首先要配置git的一些设置，这个可以用指令一个一个添加，也可以通过vscode直接编辑。输入`git config --global --edit`就可打开git的配置文件，可能安装git的时候选择了默认打开方式为vscode，输完这个指令，文件就在vscode中打开。这里一定要配置邮箱和用户名，这个应该是本地git用户的id，在提交代码的时候可以与别人区分开来![这个参数我也不知道是什么，好像没起作用](/pic/git_config.JPG)  
接下来要配置SSH，为了在github账号了添加自己的公钥（TODO SSH的具体作用还要继续学习，不配置确实行不通）。指令是`ssh-keygen -t rsa -C myemail`，其中`myemail`是git账号里的邮箱，后面终端会提示文件路径和让你输入密码，照指示做就可以。最后生成了一个.pub文件内容是公钥，复制文件内容到github账号设置里，将其作为你的ssh key。这个时候`git commit`都能用了，但`git push`行不通。这种情况是访问不到github服务器，因为我使用了clash for windows，我在chrome里访问github没问题，单终端里不行，这里修改host文件就可以，文件路径是*C:\Windows\System32\drivers\etc\hosts*，是个文本文件。然后去<https://ping.chinaz.com/>找一个好访问github.com的IP，host里添加一行`ip github.com`，其中的`ip`是网站为你找到的ip地址。  
ssh配置最终是否生效，要看`ssh -T git@github.com`指令的输出，出现successfully authenticated的字眼就是成功了，之后的push操作也不需要在输入用户名和密码了。出现这种错误就是代理或host的问题![这个参数不加不行](/pic/push_error.JPG)  
参考书籍：
《GitHub入门与实践》 大塚弘记 人民邮电出版社