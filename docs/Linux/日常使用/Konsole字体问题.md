![](/images/字体混乱.png)
之前一直有这种字体问题。

后来在telegram上求助，@Senaruk大佬回复说调fontconfig，参考下面的连接：
<https://github.com/liolok/dotfiles/blob/master/.config/fontconfig/fonts.conf>
我把链接中的内容复制到用户字体配置文件~/.config/fontconfig/fonts.conf后，再使用 `fc-cache` 命令重建 Fontconfig 的配置，就解决了问题
![](/images/Pasted%20image%2020230217143054.png)

之后有时间再细看字体配置：
<https://wiki.archlinuxcn.org/wiki/%E5%AD%97%E4%BD%93%E9%85%8D%E7%BD%AE>
