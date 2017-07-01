# ngx_http_echo_module

ԭ�ģ�[Nginxģ�鿪������](http://blog.codinglabs.org/articles/intro-of-nginx-module-development.html "Nginxģ�鿪������")

<h1><a name="section0"></a>ǰ��</h1>
<p>Nginx�ǵ�ǰ�����е�HTTP Server֮һ������W3Techs��ͳ�ƣ�Ŀǰ��������������Alexa��ǰ100�����վ�У�<a href="http://w3techs.com/technologies/overview/web_server/all" target="_blank">Nginx��ռ����Ϊ6.8%</a>����Apache��ȣ�<a href="http://www.joeandmotorboat.com/2008/02/28/apache-vs-nginx-web-server-performance-deathmatch/" target="_blank">Nginx�ڸ߲�������¾��о޴����������</a>��</p>
<p>Nginx���ڵ��͵�΢�ں���ƣ����ں˷ǳ��������ţ�ͬʱ���зǳ��ߵĿ���չ�ԡ�Nginx���������Ҫ�����������������������HTTP���ĵĳ���͸���HTTP��չģ��ķḻ��NginxԽ��Խ�౻����ȡ��Apache�������е�HTTP Server�����Σ�����Ŀǰ�Ա��ڸ���������Խ��Խ��ʹ��Nginxȡ��Apache���ݱ����˽⣬����Ѷ�����˵ȹ�˾Ҳ�������������</p>
<p>ͬʱ�������ĵ�������չģ��Ҳ��NginxԽ��Խǿ�����磬���Ա��Ĺ���ʦ���ޣ������ܣ��ʹ��������ഺ����������<a href="https://github.com/chaoslawful/lua-nginx-module" target="_blank">nginx_lua_module</a>���Խ�Lua����Ƕ�뵽Nginx�����У��Ӷ�����Lua������ǿ��Nginx����ı���������������Բ�����������ű����ԣ���PHP��Python�ȣ���ֻ��Nginx����Ϳ���ʵ�ָ���ҵ��Ĵ�����������������<a href="https://github.com/agentzh/ngx_openresty" target="_blank">ngx_openresty</a>����ͨ������<a href="http://luajit.org/" target="_blank">LuaJIT</a>���������Nginx��������һ����ȫ��Ӧ�ÿ���ƽ̨��Ŀǰ�Ա�����ƽ̨���Ʒ������ͳ�ƵĲ�Ʒ���ǻ���ngx_openresty����������ngxin_lua_module��ngx_openresty����Ȥ�����ѿ��Բο����ڹؼ����ϸ��������ӣ�������Ҳ���ܻ�дһЩ�����йص����¡�</p>
<p>���Ľ����ص��עNginxģ�鿪�����ż�������ĿǰNginx��ѧϰ���Ϸǳ��٣�����չģ�鿪����ص����ϼ���ֻ�С�<a href="http://www.evanmiller.org/nginx-modules-guide.html" target="_blank">Emiller's Guide To Nginx Module Development</a>��һ�ģ�����ʮ�־��䣬��������Nginx�汾���ݽ��������������ݿ����е��ʱ�������Ǳ������ж���ƪ���º�NginxԴ����Ļ����ϣ����Լ�ѧϰNginxģ�鿪����һ���ܽᡣ���Ľ�ͨ��һ��������ģ�鿪��ʵ������Nginxģ�鿪�����������ݡ�</p>
<p>���Ľ�����Nginx���µ�<a href="http://nginx.org/download/nginx-1.0.0.tar.gz" target="_blank">1.0.0</a>�汾������ϵͳ����ΪLinux��Ubuntu10.10����
<!--more-->
</p>
<h1><a name="section1"></a>Nginx��Ҫ</h1>
<p>����Nginx��չ��Ȼ��Ҫǰ���Ƕ�Nginx��һ�����˽⣬Ȼ�����Ĳ���������ϸ����Nginx�ķ������棬����Nginx�İ�װ�͸�����ϸ���õ����ݶ�������Nginx������Document���ҵ�������������ֻ�����������һЩ������ܻ��õ���ԭ��͸��</p>
<h2><a name="section1-1"></a>Nginx��Linux�µİ�װ������</h2>
<p>ʹ��Nginx�ĵ�һ��������NginxԴ���������1.0.0�����ص�ַΪ<a title="http://nginx.org/download/nginx-1.0.0.tar.gz" href="http://nginx.org/download/nginx-1.0.0.tar.gz">http://nginx.org/download/nginx-1.0.0.tar.gz</a>�����������tar�����ѹ��������Ŀ¼��װ������Linux��ͨ���������죬�������뽫Nginx��װ��/usr/local/nginx�£���ִ���������</p>
<pre class="prettyprint linenums">./configure --prefix=/usr/local/nginx
make
make install</pre>
<p>��װ��ɺ����ֱ��ʹ��������������Nginx��</p>
<pre class="prettyprint linenums">/usr/local/nginx/sbin/nginx</pre>
<p>NginxĬ����Deamon���������������������</p>
<pre class="prettyprint linenums">curl -i http://localhost/</pre>
<p>�Ϳ��Լ��Nginx�Ƿ��Ѿ��ɹ����С�����Ҳ�����������������<a href="http://localhost/">http://localhost/</a>��Ӧ�ÿ��Կ���Nginx�Ļ�ӭҳ���ˡ������������ֹͣNginx����ʹ�ã�</p>
<pre class="prettyprint linenums">/usr/local/nginx/sbin/nginx -s stop</pre>
<h2><a name="section1-2"></a>Nginx�����ļ������ṹ</h2>
<p>�����ļ����Կ�����Nginx����꣬Nginx����������ʱ����������ļ�������������һ�ж�����Ϊ���ǰ��������ļ��е�ָ����еģ���������Nginx������һ�����������ôNginx�������ļ����Կ�����ȫ���ĳ���ָ�</p>
<p>������һ��Nginx�����ļ���ʵ����</p>
<pre class="prettyprint linenums">#user  nobody;
worker_processes  8;
error_log  logs/error.log;
pid        logs/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  65;
    #gzip  on;

    server {
        listen       80;
        server_name  localhost;
        location / {
            root   /home/yefeng/www;
            index  index.html index.htm;
        }
        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}</pre>
<p>Nginx�����ļ��Ǵ��ı��ļ�����������κ��ı��༭����vim��emacs������ͨ��������nginx��װĿ¼��conf�£����ҵ�nginx��װ��/usr/local/nginx���������ļ�Ĭ�Ϸ���/usr/local/nginx/conf/nginx.conf��</p>
<p>���С�#����ʾ������ע�ͣ����ڱ���Ϊ��ѧϰ��չ������װ��һ��������Nginx����������ļ�û�о���̫��Ķ���</p>
<p>Nginx�������ļ�����block����ʽ��֯�ģ�һ��blockͨ��ʹ�ô����š�{}����ʾ��block��Ϊ�����㼶�����������ļ�Ϊmain�㼶���������Ĳ㼶����main�㼶�¿�����event��http�Ȳ㼶����http���ֻ���server block��server block�п��԰���location block��</p>
<p>ÿ���㼶�������Լ���ָ�Directive��������worker_processes��һ��main�㼶ָ���ָ��Nginx�����Worker�����������е�ָ��ֻ����һ���㼶�����ã���worker_processesֻ�ܴ�����main�У����е�ָ����Դ����ڶ���㼶������������£���block��̳и�block�����ã�ͬʱ�����block�������븸block��ͬ��ָ���Ḳ�ǵ���block�����á�ָ��ĸ�ʽ�ǡ�ָ���� ����1 ����2 �� ����N;����ע�������������������ո�ָ������Ҫ�ӷֺš�</p>
<p>�ڿ���Nginx HTTP��չģ������У���Ҫ�ر�ע�����main��server��location�����㼶����Ϊ��չģ��ͨ������ָ���µ�����ָ�����������㼶�С�</p>
<p>���Ҫ�ᵽ���������ļ��ǿ��԰����ģ������������ļ��С�include mime.types���Ͱ�����mine.types��������ļ������ļ�ָ���˸���HTTP Content-type��</p>
<p>һ����˵��һ��server block��ʾһ��Host���������һ��location�����һ��·��ӳ�����������block����˵��HTTP���õĺ��ġ�</p>
<p>��ͼ��Nginx�����ļ�ͨ���ṹͼʾ��</p>
<p class="picture"><img alt="" src="/uploads/pictures/intro-of-nginx-module-development/1.png"/></p>
<p>����Nginx���õĸ���������ο�Nginx�ٷ��ĵ���</p>
<h2><a name="section1-3"></a>Nginxģ�鹤��ԭ�����</h2>
<p>��Nginx����֧�ֶ���ģ�飬��HTTPģ�顢EVENTģ���MAILģ�飬����ֻ����HTTPģ�飩</p>
<p>Nginx�������Ĺ���ʵ�ʺ��٣������ӵ�һ��HTTP����ʱ����������ͨ�����������ļ����˴�����ӳ�䵽һ��location block������location�������õĸ���ָ�����������ͬ��ģ��ȥ��ɹ��������ģ����Կ���Nginx�������Ͷ������ߡ�ͨ��һ��location�е�ָ����漰һ��handlerģ��Ͷ��filterģ�飨��Ȼ�����location���Ը���ͬһ��ģ�飩��handlerģ�鸺�������������Ӧ���ݵ����ɣ���filterģ�����Ӧ���ݽ��д������Nginxģ�鿪����Ϊhandler������filter���������Ĳ�����load-balancerģ�飩����ͼչʾ��һ�γ����������Ӧ�Ĺ��̡�</p>
<p class="picture"><img alt="" src="/uploads/pictures/intro-of-nginx-module-development/2.png"/></p>
<h1><a name="section2"></a>Nginxģ�鿪��ʵս</h1>
<p>���汾��չʾһ���򵥵�Nginxģ�鿪��ȫ���̣����ǿ���һ����echo��handlerģ�飬���ģ�鹦�ܷǳ��򵥣������ա�echo��ָ�ָ���ָ��һ���ַ���������ģ����������ַ�����ΪHTTP��Ӧ�����磬���������ã�</p>
<pre class="prettyprint linenums">location /echo {
    echo &quot;hello nginx&quot;;
}</pre>
<p>�����<a href="http://hostname/echo">http://hostname/echo</a>ʱ�����hello nginx��</p>
<p>ֱ��������Ҫʵ�����������Ҫ������1�����������ļ���echoָ��������2������HTTP��װ�����HTTPͷ�ȹ�������3����������ظ��ͻ��ˡ����汾�Ľ��ֲ���������ģ��Ŀ������̡�</p>
<h2><a name="section2-1"></a>����ģ�����ýṹ</h2>
<p>����������Ҫһ���ṹ���ڴ洢�������ļ��ж����������ָ���������ģ��������Ϣ�ṹ������Nginxģ�鿪����������ṹ����������Ϊngx_http_[module-name]_[main|srv|loc]_conf_t������main��srv��loc�ֱ����ڱ�ʾͬһģ��������block�е�������Ϣ���������ǵ�echoģ��ֻ��Ҫ������loc�㼶�£���Ҫ�洢һ���ַ���������������ǿ��Զ������µ�ģ�����ã�</p>
<pre class="prettyprint linenums">typedef struct {
    ngx_str_t  ed;
} ngx_http_echo_loc_conf_t;</pre>
<p>�����ֶ�ed���ڴ洢echoָ��ָ������Ҫ������ַ�����ע������ed�����ͣ���Nginxģ�鿪����ʹ��ngx_str_t���ͱ�ʾ�ַ�����������Ͷ�����core/ngx_string�У�</p>
<pre class="prettyprint linenums">typedef struct {
    size_t      len;
    u_char     *data;
} ngx_str_t;</pre>
<p>���������ֶηֱ��ʾ�ַ����ĳ��Ⱥ�������ʼ��ַ��ע����NginxԴ�����ж��������ͽ����˱�ƶ��壬��ngx_int_tΪintptr_t�ı�ƣ�Ϊ�˱���һ�£��ڿ���Nginxģ��ʱҲӦ��ʹ����ЩNginxԴ�붨������Ͷ���Ҫʹ��Cԭ�����͡�����ngx_str_t�⣬�����������õ�nginx type�ֱ�Ϊ��</p>
<pre class="prettyprint linenums">typedef intptr_t        ngx_int_t;
typedef uintptr_t       ngx_uint_t;
typedef intptr_t        ngx_flag_t;</pre>
<p>���嶨����ο�core/ngx_config.h������intptr_t��uintptr_t��ο�C99�е�<a href="http://linux.die.net/include/stdint.h" target="_blank">stdint.h</a>��<a title="http://linux.die.net/man/3/intptr_t" href="http://linux.die.net/man/3/intptr_t">http://linux.die.net/man/3/intptr_t</a>��</p>
<h2><a name="section2-2"></a>����ָ��</h2>
<p>һ��Nginxģ����������һ�����ָ�echoģ�����һ��ָ�echo����Nginxģ��ʹ��һ��ngx_command_t�����ʾģ�����ܽ��յ�����ģ�飬����ÿһ��Ԫ�ر�ʾһ����ָ�ngx_command_t��ngx_command_s��һ����ƣ�Nginxϰ����ʹ�á�_s����׺�����ṹ�壬Ȼ��typedefһ��ͬ����_t����׺������Ϊ�˽ṹ�������������ngx_command_s������core/ngx_config_file.h�У�</p>
<pre class="prettyprint linenums">struct ngx_command_s {
    ngx_str_t             name;
    ngx_uint_t            type;
    char               *(*set)(ngx_conf_t *cf, ngx_command_t *cmd, void *conf);
    ngx_uint_t            conf;
    ngx_uint_t            offset;
    void                 *post;
};</pre>
<p>����name�Ǵ���ָ������ƣ�typeʹ�������־λ��ʽ����ָ���������ؿ���type������core/ngx_config_file.h�У�</p>
<pre class="prettyprint linenums">#define NGX_CONF_NOARGS      0x00000001
#define NGX_CONF_TAKE1       0x00000002
#define NGX_CONF_TAKE2       0x00000004
#define NGX_CONF_TAKE3       0x00000008
#define NGX_CONF_TAKE4       0x00000010
#define NGX_CONF_TAKE5       0x00000020
#define NGX_CONF_TAKE6       0x00000040
#define NGX_CONF_TAKE7       0x00000080
#define NGX_CONF_MAX_ARGS    8
#define NGX_CONF_TAKE12      (NGX_CONF_TAKE1|NGX_CONF_TAKE2)
#define NGX_CONF_TAKE13      (NGX_CONF_TAKE1|NGX_CONF_TAKE3)
#define NGX_CONF_TAKE23      (NGX_CONF_TAKE2|NGX_CONF_TAKE3)
#define NGX_CONF_TAKE123     (NGX_CONF_TAKE1|NGX_CONF_TAKE2|NGX_CONF_TAKE3)
#define NGX_CONF_TAKE1234    (NGX_CONF_TAKE1|NGX_CONF_TAKE2|NGX_CONF_TAKE3   \
        |NGX_CONF_TAKE4)
#define NGX_CONF_ARGS_NUMBER 0x000000ff
#define NGX_CONF_BLOCK       0x00000100
#define NGX_CONF_FLAG        0x00000200
#define NGX_CONF_ANY         0x00000400
#define NGX_CONF_1MORE       0x00000800
#define NGX_CONF_2MORE       0x00001000
#define NGX_CONF_MULTI       0x00002000</pre>
<p>����NGX_CONF_NOARGS��ʾ��ָ����ܲ�����NGX_CON F_TAKE1-7��ʾ��ȷ����1-7����NGX_CONF_TAKE12��ʾ����1��2��������NGX_CONF_1MORE��ʾ����һ��������NGX_CONF_FLAG��ʾ���ܡ�on|off������</p>
<p>set��һ������ָ�룬����ָ��һ������ת���������������һ���ǽ������ļ������ָ��Ĳ���ת������Ҫ�ĸ�ʽ���������ýṹ�塣NginxԤ������һЩת�����������Է������ǵ��ã���Щ����������core/ngx_conf_file.h�У�һ���ԡ�_slot����β������ngx_conf_set_flag_slot����on��off��ת��Ϊ��1��0��������ngx_conf_set_str_slot�����ַ���ת��Ϊngx_str_t��</p>
<p>conf����ָ��Nginx��Ӧ�����ļ��ڴ���ʵ��ַ��һ�����ͨ�����ó���ָ������NGX_HTTP_LOC_CONF_OFFSET��offsetָ������ָ��Ĳ�����ƫ������</p>
<p>������echoģ���ָ��壺</p>
<pre class="prettyprint linenums">static ngx_command_t  ngx_http_echo_commands[] = {
    { ngx_string(&quot;echo&quot;),
        NGX_HTTP_LOC_CONF|NGX_CONF_TAKE1,
        ngx_http_echo,
        NGX_HTTP_LOC_CONF_OFFSET,
        offsetof(ngx_http_echo_loc_conf_t, ed),
        NULL },
        ngx_null_command
};</pre>
<p>ָ���������������Ϊngx_http_[module-name]_commands��ע���������һ��Ԫ��Ҫ��ngx_null_command������</p>
<p>����ת�������Ĵ���Ϊ��</p>
<pre class="prettyprint linenums">static char *
ngx_http_echo(ngx_conf_t *cf, ngx_command_t *cmd, void *conf)
{
    ngx_http_core_loc_conf_t  *clcf;
    clcf = ngx_http_conf_get_module_loc_conf(cf, ngx_http_core_module);
    clcf-&gt;handler = ngx_http_echo_handler;
    ngx_conf_set_str_slot(cf,cmd,conf);
    return NGX_CONF_OK;
}</pre>
<p>����������˵���ngx_conf_set_str_slotת��echoָ��Ĳ����⣬�����޸��˺���ģ�����ã�Ҳ�������location�����ã�������handler�滻Ϊ���Ǳ�д��handler��ngx_http_echo_handler�������������˴�location��Ĭ��handler��ʹ��ngx_http_echo_handler����HTTP��Ӧ��</p>
<h2><a name="section2-3"></a>�����ϲ�������Ϣ</h2>
<p>��һ���Ƕ���ģ��Context��</p>
<p>����������Ҫ����һ��ngx_http_module_t���͵Ľṹ���������������Ϊngx_http_[module-name]_module_ctx������ṹ��Ҫ���ڶ������Hook������������echoģ���context�ṹ��</p>
<pre class="prettyprint linenums">static ngx_http_module_t  ngx_http_echo_module_ctx = {
    NULL,                                  /* preconfiguration */
    NULL,                                  /* postconfiguration */
    NULL,                                  /* create main configuration */
    NULL,                                  /* init main configuration */
    NULL,                                  /* create server configuration */
    NULL,                                  /* merge server configuration */
    ngx_http_echo_create_loc_conf,         /* create location configration */
    ngx_http_echo_merge_loc_conf           /* merge location configration */
};</pre>
<p>���Կ���һ����8��Hookע��㣬�ֱ���ڲ�ͬʱ�̱�Nginx���ã��������ǵ�ģ���������location�����ｫ����Ҫ��ע�����ΪNULL���ɡ�����create_loc_conf���ڳ�ʼ��һ�����ýṹ�壬��Ϊ���ýṹ������ڴ�ȹ�����merge_loc_conf���ڽ��丸block��������Ϣ�ϲ����˽ṹ���У�Ҳ����ʵ�����õļ̳С������������ᱻNginx�Զ����á�ע���������������ngx_http_[module-name]_[create|merge]_[main|srv|loc]_conf��</p>
<p>������echoģ��������������Ĵ��룺
<pre class="prettyprint linenums">static void *
ngx_http_echo_create_loc_conf(ngx_conf_t *cf)
{
    ngx_http_echo_loc_conf_t  *conf;
    conf = ngx_pcalloc(cf-&gt;pool, sizeof(ngx_http_echo_loc_conf_t));
    if (conf == NULL) {
        return NGX_CONF_ERROR;
    }
    conf-&gt;ed.len = 0;
    conf-&gt;ed.data = NULL;
    return conf;
}
static char *
ngx_http_echo_merge_loc_conf(ngx_conf_t *cf, void *parent, void *child)
{
    ngx_http_echo_loc_conf_t *prev = parent;
    ngx_http_echo_loc_conf_t *conf = child;
    ngx_conf_merge_str_value(conf-&gt;ed, prev-&gt;ed, &quot;&quot;);
    return NGX_CONF_OK;
}</pre>
<p>����ngx_pcalloc������Nginx�ڴ���з���һ��ռ䣬��pcalloc��һ����װ��ʹ��ngx_pcalloc������ڴ�ռ䲻���ֹ�free��Nginx�����й������ʵ��Ƿ��ͷš�</p>
<p>create_loc_conf�½�һ��ngx_http_echo_loc_conf_t�������ڴ棬����ʼ�����е����ݣ�Ȼ�󷵻�����ṹ��ָ�룻merge_loc_conf����block���������Ϣ�ϲ���create_loc_conf�½������ýṹ���С�</p>
<p>����ngx_conf_merge_str_value����һ������������һ���꣬�䶨����core/ngx_conf_file.h�У�</p>
<pre class="prettyprint linenums">#define ngx_conf_merge_str_value(conf, prev, default)                        \
    if (conf.data == NULL) {                                                 \
        if (prev.data) {                                                     \
            conf.len = prev.len;                                             \
            conf.data = prev.data;                                           \
        } else {                                                             \
            conf.len = sizeof(default) - 1;                                  \
            conf.data = (u_char *) default;                                  \
        }                                                                    \
    }</pre>
<p>ͬʱ���Կ�����core/ngx_conf_file.h�������˺ܶ�merge value�ĺ�����merge�������ݡ����ǵ���Ϊ�Ƚ����ƣ�ʹ��prev���conf�����prev������Ϊ����ʹ��default��䡣</p>
<h2><a name="section2-4"></a>��дHandler</h2>
<p>����Ĺ����Ǳ�дhandler��handler����˵��ģ���������ɻ�Ĵ��룬����Ҫ����������ְ��</p>
<p>����ģ�����á�</p>
<p>������ҵ��</p>
<p>����HTTP header��</p>
<p>����HTTP body��</p>
<p>����������echoģ��Ĵ��룬Ȼ��ͨ����������ķ�ʽ�������ʵ�����Ĳ�����һ��Ĵ���Ƚϸ��ӣ�</p>
<pre class="prettyprint linenums">static ngx_int_t
ngx_http_echo_handler(ngx_http_request_t *r)
{
    ngx_int_t rc;
    ngx_buf_t *b;
    ngx_chain_t out;
    ngx_http_echo_loc_conf_t *elcf;
    elcf = ngx_http_get_module_loc_conf(r, ngx_http_echo_module);
    if(!(r-&gt;method &amp; (NGX_HTTP_HEAD|NGX_HTTP_GET|NGX_HTTP_POST)))
    {
        return NGX_HTTP_NOT_ALLOWED;
    }
    r-&gt;headers_out.content_type.len = sizeof(&quot;text/html&quot;) - 1;
    r-&gt;headers_out.content_type.data = (u_char *) &quot;text/html&quot;;
    r-&gt;headers_out.status = NGX_HTTP_OK;
    r-&gt;headers_out.content_length_n = elcf-&gt;ed.len;
    if(r-&gt;method == NGX_HTTP_HEAD)
    {
        rc = ngx_http_send_header(r);
        if(rc != NGX_OK)
        {
            return rc;
        }
    }
    b = ngx_pcalloc(r-&gt;pool, sizeof(ngx_buf_t));
    if(b == NULL)
    {
        ngx_log_error(NGX_LOG_ERR, r-&gt;connection-&gt;log, 0, &quot;Failed to allocate response buffer.&quot;);
        return NGX_HTTP_INTERNAL_SERVER_ERROR;
    }
    out.buf = b;
    out.next = NULL;
    b-&gt;pos = elcf-&gt;ed.data;
    b-&gt;last = elcf-&gt;ed.data + (elcf-&gt;ed.len);
    b-&gt;memory = 1;
    b-&gt;last_buf = 1;
    rc = ngx_http_send_header(r);
    if(rc != NGX_OK)
    {
        return rc;
    }
    return ngx_http_output_filter(r, &amp;out);
}</pre>
<p>handler�����һ��ngx_http_request_tָ�����͵Ĳ������������ָ��һ��ngx_http_request_t�ṹ�壬�˽ṹ��洢�����HTTP�����һЩ��Ϣ������ṹ������http/ngx_http_request.h�У�</p>
<pre class="prettyprint linenums">struct ngx_http_request_s {
    uint32_t                          signature;         /* &quot;HTTP&quot; */
    ngx_connection_t                 *connection;
    void                            **ctx;
    void                            **main_conf;
    void                            **srv_conf;
    void                            **loc_conf;
    ngx_http_event_handler_pt         read_event_handler;
    ngx_http_event_handler_pt         write_event_handler;
#if (NGX_HTTP_CACHE)
    ngx_http_cache_t                 *cache;
#endif
    ngx_http_upstream_t              *upstream;
    ngx_array_t                      *upstream_states;
    /* of ngx_http_upstream_state_t */
    ngx_pool_t                       *pool;
    ngx_buf_t                        *header_in;
    ngx_http_headers_in_t             headers_in;
    ngx_http_headers_out_t            headers_out;
    ngx_http_request_body_t          *request_body;
    time_t                            lingering_time;
    time_t                            start_sec;
    ngx_msec_t                        start_msec;
    ngx_uint_t                        method;
    ngx_uint_t                        http_version;
    ngx_str_t                         request_line;
    ngx_str_t                         uri;
    ngx_str_t                         args;
    ngx_str_t                         exten;
    ngx_str_t                         unparsed_uri;
    ngx_str_t                         method_name;
    ngx_str_t                         http_protocol;
    ngx_chain_t                      *out;
    ngx_http_request_t               *main;
    ngx_http_request_t               *parent;
    ngx_http_postponed_request_t     *postponed;
    ngx_http_post_subrequest_t       *post_subrequest;
    ngx_http_posted_request_t        *posted_requests;
    ngx_http_virtual_names_t         *virtual_names;
    ngx_int_t                         phase_handler;
    ngx_http_handler_pt               content_handler;
    ngx_uint_t                        access_code;
    ngx_http_variable_value_t        *variables;
    /* ... */
}</pre>
<p>����ngx_http_request_s����Ƚϳ���������ֻ��ȡ��һ���֡����Կ�������������uri��args��request_body��HTTP������Ϣ��������Ҫ�ر�ע��ļ����ֶ���headers_in��headers_out��chain�����Ƿֱ��ʾrequest header��response header��������ݻ���������������������Nginx I/O�е���Ҫ���ݣ�����ᵥ�����ܣ���</p>
<p>��һ���ǻ�ȡģ��������Ϣ����һ��ֻҪ��ʹ��ngx_http_get_module_loc_conf�Ϳ����ˡ�</p>
<p>�ڶ����ǹ����߼�����Ϊechoģ��ǳ��򵥣�ֻ�Ǽ����һ���ַ�������������û�й����߼����롣</p>
<p>������������response header��Header���ݿ���ͨ�����headers_outʵ�֣���������ֻ������Content-type��Content-length�Ȼ������ݣ�ngx_http_headers_out_t���������п������õ�HTTP Response Header��Ϣ��</p>
<pre class="prettyprint linenums">typedef struct {
    ngx_list_t                        headers;
    ngx_uint_t                        status;
    ngx_str_t                         status_line;
    ngx_table_elt_t                  *server;
    ngx_table_elt_t                  *date;
    ngx_table_elt_t                  *content_length;
    ngx_table_elt_t                  *content_encoding;
    ngx_table_elt_t                  *location;
    ngx_table_elt_t                  *refresh;
    ngx_table_elt_t                  *last_modified;
    ngx_table_elt_t                  *content_range;
    ngx_table_elt_t                  *accept_ranges;
    ngx_table_elt_t                  *www_authenticate;
    ngx_table_elt_t                  *expires;
    ngx_table_elt_t                  *etag;
    ngx_str_t                        *override_charset;
    size_t                            content_type_len;
    ngx_str_t                         content_type;
    ngx_str_t                         charset;
    u_char                           *content_type_lowcase;
    ngx_uint_t                        content_type_hash;
    ngx_array_t                       cache_control;
    off_t                             content_length_n;
    time_t                            date_time;
    time_t                            last_modified_time;
} ngx_http_headers_out_t;</pre>
<p>���ﲢ����������HTTPͷ��Ϣ�������Ҫ����ʹ��agentzh��������������Nginxģ��<a href="http://wiki.nginx.org/HttpHeadersMoreModule" target="_blank">HttpHeadersMore</a>��ָ����ָ�������Headerͷ��Ϣ��</p>
<p>���ú�ͷ��Ϣ��ʹ��ngx_http_send_header�Ϳ��Խ�ͷ��Ϣ�����ngx_http_send_header����һ��ngx_http_request_t���͵Ĳ�����</p>
<p>���Ĳ�Ҳ������Ҫ��һ�������Response body����������Ҫ�˽�Nginx��I/O���ƣ�Nginx����handlerһ�β���һ����������Բ�����Σ�Nginx�������֯��һ��������ṹ�������е�ÿ���ڵ���һ��chain_t��������core/ngx_buf.h��</p>
<pre class="prettyprint linenums">struct ngx_chain_s {
    ngx_buf_t    *buf;
    ngx_chain_t  *next;
};</pre>
<p>����ngx_chain_t��ngx_chain_s�ı�����bufΪĳ�����ݻ�������ָ�룬nextָ����һ������ڵ㣬���Կ�������һ���ǳ��򵥵�����ngx_buf_t�Ķ���Ƚϳ����Һܸ��ӣ�����Ͳ��������ˣ������вο�core/ngx_buf.h��ngx_but_t�бȽ���Ҫ����pos��last���ֱ��ʾҪ�������������ڴ��е���ʼ��ַ�ͽ�β��ַ���������ǽ��������ַ�������ȥ��last_buf��һ��λ����Ϊ1��ʾ�˻����������������һ��Ԫ�أ�Ϊ0��ʾ���滹��Ԫ�ء���Ϊ����ֻ��һ�����ݣ����Ի�����������ֻ��һ���ڵ㣬�����Ҫ����������ݿɽ��������ݷ��벻ͬ����������뵽������ͼչʾ��Nginx��������Ľṹ��</p>
<p class="picture"><img alt="" src="/uploads/pictures/intro-of-nginx-module-development/3.png"/></p>
<p>��������׼���ú���ngx_http_output_filter�Ϳ�������ˣ����͵�filter���и��ֹ��˴�����ngx_http_output_filter�ĵ�һ������Ϊngx_http_request_t�ṹ���ڶ���Ϊ����������ʼ��ַ&amp;out��ngx_http_out_put_filter�������������������ݡ�</p>
<p>���Ͼ���handler�����й���������������������������handler���롣</p>
<h2>���Nginx Module</h2>
<p>���������Nginxģ���������Ŀ���������ǽ���Щ��������ˡ�һ��Nginxģ�鱻����Ϊһ��ngx_module_t�ṹ������ṹ���ֶκܶ࣬������ͷ�ͽ�β�����ֶ�һ�����ͨ��Nginx���õĺ�ȥ��䣬����������echoģ���ģ�����嶨�壺</p>
<pre class="prettyprint linenums">ngx_module_t  ngx_http_echo_module = {
    NGX_MODULE_V1,
    &amp;ngx_http_echo_module_ctx,             /* module context */
    ngx_http_echo_commands,                /* module directives */
    NGX_HTTP_MODULE,                       /* module type */
    NULL,                                  /* init master */
    NULL,                                  /* init module */
    NULL,                                  /* init process */
    NULL,                                  /* init thread */
    NULL,                                  /* exit thread */
    NULL,                                  /* exit process */
    NULL,                                  /* exit master */
    NGX_MODULE_V1_PADDING
};</pre>
<p>��ͷ�ͽ�β�ֱ���NGX_MODULE_V1��NGX_MODULE_V1_PADDING ����������ֶΣ��Ͳ�ȥ��ˡ�������Ҫ��Ҫ�������Ϣ���ϵ���������Ϊcontext��ָ�����顢ģ�������Լ������ض��¼��Ļص�������������Ҫ������ΪNULL�����������ݻ��ǱȽϺ����ģ�ע�����ǵ�echo��һ��HTTPģ�飬��������������NGX_HTTP_MODULE�������������ͻ���NGX_EVENT_MODULE���¼�����ģ�飩��NGX_MAIL_MODULE���ʼ�ģ�飩��</p>
<p>����������echoģ���д���ˣ��������echoģ����������룺</p>
<pre class="prettyprint linenums">/*
* Copyright (C) Eric Zhang
*/
#include &lt;ngx_config.h&gt;
#include &lt;ngx_core.h&gt;
#include &lt;ngx_http.h&gt;
/* Module config */
typedef struct {
    ngx_str_t  ed;
} ngx_http_echo_loc_conf_t;
static char *ngx_http_echo(ngx_conf_t *cf, ngx_command_t *cmd, void *conf);
static void *ngx_http_echo_create_loc_conf(ngx_conf_t *cf);
static char *ngx_http_echo_merge_loc_conf(ngx_conf_t *cf, void *parent, void *child);
/* Directives */
static ngx_command_t  ngx_http_echo_commands[] = {
    { ngx_string(&quot;echo&quot;),
        NGX_HTTP_LOC_CONF|NGX_CONF_TAKE1,
        ngx_http_echo,
        NGX_HTTP_LOC_CONF_OFFSET,
        offsetof(ngx_http_echo_loc_conf_t, ed),
        NULL },
        ngx_null_command
};
/* Http context of the module */
static ngx_http_module_t  ngx_http_echo_module_ctx = {
    NULL,                                  /* preconfiguration */
    NULL,                                  /* postconfiguration */
    NULL,                                  /* create main configuration */
    NULL,                                  /* init main configuration */
    NULL,                                  /* create server configuration */
    NULL,                                  /* merge server configuration */
    ngx_http_echo_create_loc_conf,         /* create location configration */
    ngx_http_echo_merge_loc_conf           /* merge location configration */
};
/* Module */
ngx_module_t  ngx_http_echo_module = {
    NGX_MODULE_V1,
    &amp;ngx_http_echo_module_ctx,             /* module context */
    ngx_http_echo_commands,                /* module directives */
    NGX_HTTP_MODULE,                       /* module type */
    NULL,                                  /* init master */
    NULL,                                  /* init module */
    NULL,                                  /* init process */
    NULL,                                  /* init thread */
    NULL,                                  /* exit thread */
    NULL,                                  /* exit process */
    NULL,                                  /* exit master */
    NGX_MODULE_V1_PADDING
};
/* Handler function */
static ngx_int_t
ngx_http_echo_handler(ngx_http_request_t *r)
{
    ngx_int_t rc;
    ngx_buf_t *b;
    ngx_chain_t out;
    ngx_http_echo_loc_conf_t *elcf;
    elcf = ngx_http_get_module_loc_conf(r, ngx_http_echo_module);
    if(!(r-&gt;method &amp; (NGX_HTTP_HEAD|NGX_HTTP_GET|NGX_HTTP_POST)))
    {
        return NGX_HTTP_NOT_ALLOWED;
    }
    r-&gt;headers_out.content_type.len = sizeof(&quot;text/html&quot;) - 1;
    r-&gt;headers_out.content_type.data = (u_char *) &quot;text/html&quot;;
    r-&gt;headers_out.status = NGX_HTTP_OK;
    r-&gt;headers_out.content_length_n = elcf-&gt;ed.len;
    if(r-&gt;method == NGX_HTTP_HEAD)
    {
        rc = ngx_http_send_header(r);
        if(rc != NGX_OK)
        {
            return rc;
        }
    }
    b = ngx_pcalloc(r-&gt;pool, sizeof(ngx_buf_t));
    if(b == NULL)
    {
        ngx_log_error(NGX_LOG_ERR, r-&gt;connection-&gt;log, 0, &quot;Failed to allocate response buffer.&quot;);
        return NGX_HTTP_INTERNAL_SERVER_ERROR;
    }
    out.buf = b;
    out.next = NULL;
    b-&gt;pos = elcf-&gt;ed.data;
    b-&gt;last = elcf-&gt;ed.data + (elcf-&gt;ed.len);
    b-&gt;memory = 1;
    b-&gt;last_buf = 1;
    rc = ngx_http_send_header(r);
    if(rc != NGX_OK)
    {
        return rc;
    }
    return ngx_http_output_filter(r, &amp;out);
}
static char *
ngx_http_echo(ngx_conf_t *cf, ngx_command_t *cmd, void *conf)
{
    ngx_http_core_loc_conf_t  *clcf;
    clcf = ngx_http_conf_get_module_loc_conf(cf, ngx_http_core_module);
    clcf-&gt;handler = ngx_http_echo_handler;
    ngx_conf_set_str_slot(cf,cmd,conf);
    return NGX_CONF_OK;
}
static void *
ngx_http_echo_create_loc_conf(ngx_conf_t *cf)
{
    ngx_http_echo_loc_conf_t  *conf;
    conf = ngx_pcalloc(cf-&gt;pool, sizeof(ngx_http_echo_loc_conf_t));
    if (conf == NULL) {
        return NGX_CONF_ERROR;
    }
    conf-&gt;ed.len = 0;
    conf-&gt;ed.data = NULL;
    return conf;
}
static char *
ngx_http_echo_merge_loc_conf(ngx_conf_t *cf, void *parent, void *child)
{
    ngx_http_echo_loc_conf_t *prev = parent;
    ngx_http_echo_loc_conf_t *conf = child;
    ngx_conf_merge_str_value(conf-&gt;ed, prev-&gt;ed, &quot;&quot;);
    return NGX_CONF_OK;
}</pre>
<h1><a name="section3"></a>Nginxģ��İ�װ</h1>
<p>Nginx��֧�ֶ�̬����ģ�飬���԰�װģ����Ҫ��ģ�������NginxԴ����������±��롣��װģ��Ĳ������£�</p>
<p>1����дģ��config�ļ�������ļ���Ҫ���ں�ģ��Դ�����ļ�����ͬһĿ¼�¡��ļ��������£�</p>
<pre class="prettyprint linenums">ngx_addon_name=ģ����������
HTTP_MODULES=&quot;$HTTP_MODULES ģ����������&quot;
NGX_ADDON_SRCS=&quot;$NGX_ADDON_SRCS $ngx_addon_dir/Դ�����ļ���&quot;</pre>
<p>2������NginxԴ���룬ʹ������������밲װ</p>
<pre class="prettyprint linenums">./configure --prefix=��װĿ¼ --add-module=ģ��Դ�����ļ�Ŀ¼
make
make install</pre>
<p>��������ɰ�װ�ˣ����磬�ҵ�Դ�����ļ�����/home/yefeng/ngxdev/ngx_http_echo�£��ҵ�config�ļ�Ϊ��</p>
<pre class="prettyprint linenums">ngx_addon_name=ngx_http_echo_module
HTTP_MODULES=&quot;$HTTP_MODULES ngx_http_echo_module&quot;
NGX_ADDON_SRCS=&quot;$NGX_ADDON_SRCS $ngx_addon_dir/ngx_http_echo_module.c&quot;</pre>
<p>���밲װ����Ϊ��</p>
<pre class="prettyprint linenums">./configure --prefix=/usr/local/nginx --add-module=/home/yefeng/ngxdev/ngx_http_echo
make
sudo make install</pre>
<p>����echoģ��ͱ���װ���ҵ�Nginx���ˣ��������һ�£��޸������ļ�����������һ�����ã�</p>
<pre class="prettyprint linenums">location /echo {
    echo &quot;This is my first nginx module!!!&quot;;
}</pre>
<p>Ȼ����curl����һ�£�</p>
<pre class="prettyprint linenums">curl -i http://localhost/echo</pre>
<p>������£�</p>
<p class="picture"><img alt="" src="/uploads/pictures/intro-of-nginx-module-development/4.png"/></p>
<p>���Կ���ģ���Ѿ����������ˣ�Ҳ������������д���ַ���Ϳ��Կ��������</p>
<p class="picture"><img alt="" src="/uploads/pictures/intro-of-nginx-module-development/5.png"/></p>
<h1><a name="section4"></a>�������ѧϰ</h1>
<p>����ֻ�Ǽ�Ҫ������Nginxģ��Ŀ������̣�����ƪ����ԭ�򣬲�������㵽����ΪĿǰNginx��ѧϰ���Ϻ��٣��������ϣ��������ѧϰNginx��ԭ��ģ�鿪������ô�Ķ�Դ��������õİ취����NginxԴ�����core/�·���Nginx�ĺ��Ĵ��룬�����Nginx���ڲ��������Ʒǳ��а�����http/Ŀ¼����Nginx HTTP��ص�ʵ�֣�http/module�·��д�������httpģ�飬�ɹ�����ѧϰģ��Ŀ�����������<a title="http://wiki.nginx.org/3rdPartyModules" href="http://wiki.nginx.org/3rdPartyModules">http://wiki.nginx.org/3rdPartyModules</a>���д�������ĵ�����ģ�飬Ҳ�Ƿǳ��õ�ѧϰ���ϡ�</p>
<p>���������������ʻ�ӭ�����ʼ���<a href="mailto:ericzhang.buaa@gmail.com">ericzhang.buaa@gmail.com</a>��ϣ�����Ķ�����������������</p>
<h1><a name="section5"></a>�ο�����</h1>
<p>[1] Evan Miller, Emiller's Guide To Nginx Module Development. <a title="http://www.evanmiller.org/nginx-modules-guide.html" href="http://www.evanmiller.org/nginx-modules-guide.html">http://www.evanmiller.org/nginx-modules-guide.html</a>, 2009</p>
<p>[2] <a title="http://wiki.nginx.org/Configuration" href="http://wiki.nginx.org/Configuration">http://wiki.nginx.org/Configuration</a></p>
<p>[3] Cl��ment Nedelcu, Nginx Http Server. Packt Publishing, 2010</p>
