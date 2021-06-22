在修改/etc/profile的PATH路径后，命令终端的所有命令均失效，此时在命令行中输入：export PATH=/usr/bin:/usr/sbin:/bin:/sbin 

然后vim /etc/profile，将修改的PATH还原，然后回到命令终端，输入source /etc/profile
