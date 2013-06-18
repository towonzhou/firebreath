firebreath下载和部署
====                                                                                                                                                                      
1. 下载fireBreath稳定版  
`git clone git://github.com/firebreath/FireBreath.git -b firebreath-1.7 firebreath-1.7`  
2. `cd firebreath-1.7`  
3. 确定你的python版本为2.7 (python -V)  
    如果python版本为3.x  
    1. sudo unlink /usr/bin/pyhon  
    2. sudo ln -s /usr/bin/python2.7 /usr/bin/python  
4. 运行fbgen.py,  
    `python fbgen.py`  
你会看到一些交互选项,如 Plugin Name[]: zhou, 
(中括号中有的为默认值,直接回车即可)   
(这个脚本会做一些必要的配置,拷贝fbgen/src/目录下的文件到projects下,
重命名Template.*为(PluginName).*等)  
5. 根据系统运行  
    `./prepmake.sh;./prepcodeblocks.sh;./prepeclipse.sh`  
6. 进入build目录  
    `cd build`  
7. `make`
8. `sudo cp ./bin/zhou/npzhou.so /usr/lib/mozilla/plugins`  
9. 重启chrome  
10. 用浏览器打开build/projects/zhou/gen/FBControl.htm即可运行插件 

代码示例
====
1. 申明和绑定get_uuid  
vim projects/zhou/zhouAPI.h  
`registerProperty("version",make_property(this,&zhouAPI::get_version));`  
//在这一行的下面加上  
`registerProperty("uuid",make_property(this,&zhouAPI::get_uuid));`  
....  
`std::string get_version();`  
//在这一行后面加上申明   
`std::string get_uuid();`

2. get_uuid定义  
vim projects/zhou/zhouAPI.cpp  
<pre><code>
std::string zhouAPI::get_uuid()   
{
    char uuid[128] = "";
    FILE* fp = fopen("/var/lib/yum/uuid", "r");
    if (!fp) {
        return ""; 
    }
    if (fgets(uuid, sizeof uuid - 1, fp) == NULL) {
        fclose(fp);
        return "";
    }
    fclose(fp);
    return std::string(uuid);
}  
</code></pre>
3. make  
4. sudo cp ./bin/zhou/npzhou.so /usr/lib/mozilla/plugins
5. 就可以在页面中的调用plugin().uuid来获得本机的uuid了
