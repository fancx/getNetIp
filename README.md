# getNetIp
获取本机外网的ip
#include <string>
#include <tchar.h>
#include <winsock2.h>
#pragma comment (lib, "ws2_32.lib")  //加载 ws2_32.dll
#include <Urlmon.h>
#include <Wininet.h>
#pragma comment(lib,"Urlmon.lib")
#pragma comment(lib, "Wininet.lib")

#include <iostream>
using namespace std;

string getNetIp()
{
	char retIp[32] = {0};
	const char *pFileName = "d:/tmp.html"; 

	// 从多个http域名获取机器使用的外网ip，若获取失败则使用后面的其他域名
	const string domains[] = {"http://bot.whatismyipaddress.com", "http://api.ipify.org", 
		"http://icanhazip.com", "http://checkip.amazonaws.com"};
	int num = sizeof(domains) / sizeof(domains[0]);

	for (int i = 0; i <= num - 1; i++)
	{
		const char * domain = domains[i].c_str();
		DeleteUrlCacheEntryA(domain);//清空缓存，否则服务器上的文件修改后，无法下载最新的文件
		if (URLDownloadToFileA(NULL, domain, pFileName, 0, NULL)==S_OK)
		{
			FILE *fp = fopen(pFileName, "r");
			if(fp)
			{
				fgets(retIp,32,fp);
				fclose(fp);
				remove(pFileName);
				break;
			}
		}
	}
	return string(retIp);
}

int main()
{
	cout<<getNetIp()<<endl;
	system("pause");
	return 0;
}
