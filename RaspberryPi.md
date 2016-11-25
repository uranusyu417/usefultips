#Raspberry Pi相关
##安装VNC，VNC Viewer连接后分辨率特别小问题
这种情况是可能原因之一：RP没有连接任何显示器，这种情况要做如下配置([参考此链接所述](https://support.realvnc.com/knowledgebase/article/View/523))
>To set a certain screen resolution for your Pi, specify the following settings in /boot/config.txt
- hdmi_force_hotplug=1
- hdmi_ignore_edid=0xa5000080  	 
- hdmi_group=2 	 
- hdmi_mode=16
