#Discuz!����SSRF+����Ӧ�ô���ִ��©��������PoC
##���
```
Desc : Discuz_ssrf_redis_codeexec.

References : https://www.seebug.org/vuldb/ssvid-91879.
```
##Docker+Pentest

###©������

####©������
©������ͨ��ssrf������redis��������ȫ�ֱ�����ֵ�������������ִ�С��ļ���source\function\function_core.php(1080G)
```
function output_replace($content) {
		...
		$content = preg_replace($_G['setting']['output']['preg']['search'], $_G['setting']['output']['preg']['replace'], $content);}
	return $content;
}
```
��dz����ʹ�û���󣬳�ʼ��ʱ��ѻ������ݼ���ȫ�ֱ���$_G��д��*_setting������/discuz/forum.php?mod=ajax&inajax=yes&action=getthreadtypes��ֱ��Getshell��
####����

Ubuntu 14.04 + LNMP + redis /mem+ Discuz X3.2!
```
./addons.sh install memcached #lnmp��װmem
./addons.sh install redis     #lnmp��װredis

```
Windows 7 + phpStudy + redis-cli.exe/memcached + Discuz X3.2!
����php.ini
```
extension=php_igbinary.dll
extension=php_redis.dll
extension=php_memcache.dll
```
Kali(Debain) + Docker(mysql + redis + skyzhou/docker-discuz)
```bash
docker run --name dz-mysql -e MYSQL_ROOT_PASSWORD=root -d mysql

docker run --name dz-redis -d yfix/redis

docker run --name dz-ssrf --link dz-mysql:mysql -p 8888:80 -d dz-redis-init apache2 "-DFOREGROUND"
```
����127.0.0.1:8888����Discuz!�İ�װ����config/config_global.php��redis�ĵ�ַ�Ͷ˿ڸ�Ϊredis�����ĵ�ַ�Ͷ˿ڣ��ں�̨�� ȫ�� -> �����Ż� -> �ڴ��Ż� �в鿴redis�Ƿ����ã�������������ɡ�

####�����е�����
* Dz��ssrfһ��������ִ�У�ֱ��getshell
* Redis��δ��Ȩ���ʣ�Ƕ��Lua�ű����ᵼ�´���ִ��ֱ��дshell/˽Կ
* ...

###�޸����
####Discuz��Dz��SSRF����

* ��ʱ����Dz�汾����������ʹ�ò������������
* �޸�function_core.php�ļ�,�滻����
```
if (preg_match("(/|#|\+|%).*(/|#|\+|%)e", $_G['setting']['output']['preg']['search']) !== FALSE) { die("request error"); } 
	$content = preg_replace($_G['setting']['output']['preg']['search'], $_G['setting']['output']['preg']['replace'], $content);
```
####Redisδ��Ȩ����

* ����bindѡ��޶���������Redis��������IP���޸� Redis ��Ĭ�϶˿�6379
* ������֤��Ҳ����AUTH���������룬����������ķ�ʽ������Redis�����ļ���
* ����rename-command ������ ��RENAME_CONFIG����������ʹ����δ��Ȩ���ʣ�Ҳ�ܹ���������ʹ��config ָ��Ӵ��Ѷ�
* ����Ϣ��Redis���߱�ʾ���Ὺ����real user����������ͨ�û���adminȨ�ޣ���ͨ�û����ᱻ��ֹ����ĳЩ�����config
* ������https://www.seebug.org/monster/?vul_id=89715

##PoCʹ��

ʹ��pocsuite���
Usage:
```
pocsuite -r dz_redis_exec.py -u url --verify
pocsuite -r dz_redis_exec.py -u url --attack
```
�������Ծ�success!

##�ο�

* http://pocsuite.org/
* https://github.com/imp0wd3r
* https://github.com/C1tas
* http://pan.baidu.com/s/1dFHOQUt
