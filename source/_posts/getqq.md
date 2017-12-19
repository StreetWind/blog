---
title: "进程查找QQ号码"
layout: "post"
categories:
  - 学习
tags:
  - 学习
keywords:
  - 进程查找QQ号码
  - 内存抓取QQ号码
  - C语言抓取QQ号码
date: "2017-12-19 11:19"
updated: "2017-12-19 11:19"
abbrlink:
comments:
---
## 进程抓取QQ号码
前天翻硬盘的时候，不知道什么时候写的一个C语言版本的通过进程抓取内存关键后面的QQ号码
原理很简单:
1. 获取系统所有进程，查找QQ进程ID。
2. 通过进程ID获取应用的内存空间。
3. 根据内存关键字查找关键字后面的QQ号码。

![](https://ws1.sinaimg.cn/large/006VAXiDgy1fmlx6bcz2sj30r70d6mx1.jpg)

```C
#include "stdafx.h"
#include <stdio.h>
#include <windows.h>
#include <tlhelp32.h>
#include <iostream>
//#include <iostream.h>

void GetQQ(DWORD dwQQID);
int main(int argc, char* argv[])
{
	PROCESSENTRY32 pe;
	DWORD id = 0;
	DWORD ids[5];
	int idleng =0;
	HANDLE hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS,0);
	pe.dwSize = sizeof(PROCESSENTRY32);
	if( !Process32First(hSnapshot,&pe) )
		return 0;

	do
	{
		pe.dwSize = sizeof(PROCESSENTRY32);
		if( Process32Next(hSnapshot,&pe)==FALSE )
			break;
		if(strcmp(pe.szExeFile,"QQ.exe") == 0)
		{

			ids[idleng] =pe.th32ProcessID;
			idleng++;
			//break;
		}

	} while(1);

	CloseHandle(hSnapshot);

	for (int k=0;k<idleng;k++)
	{

		GetQQ(ids[k]);

	}

	getchar();
	return 0;
}


void GetQQ(DWORD dwQQID)
{

	HANDLE hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, dwQQID); //打开进程


	if (hProcess !=NULL)
	{
		SuspendThread(hProcess);

		DWORD dwBaseAddress;
		MEMORY_BASIC_INFORMATION mbi;
		char   process_mem[ 4096 ]  =   { 0 } ;
		DWORD number_of_bytes_read  =   0 ;
		SYSTEM_INFO si;
		GetSystemInfo( & si);
		int s =0;
		char strqqlent[50];
		char GetQQKey[] ="\\Msg3.0.db";

		dwBaseAddress  =  (DWORD)si.lpMinimumApplicationAddress;
		while (dwBaseAddress  <  (DWORD)si.lpMaximumApplicationAddress)
		{
			mbi.BaseAddress  =  (LPVOID)dwBaseAddress;
			VirtualQueryEx(hProcess, (LPVOID)dwBaseAddress,  & mbi,  sizeof (mbi));
			dwBaseAddress  =  (DWORD)mbi.BaseAddress  +  mbi.RegionSize;
			if (mbi.State  !=  MEM_COMMIT  ||  mbi.AllocationProtect  !=  PAGE_READWRITE)  // 跳过未分配或不可读写的区域
			{
				continue ;
			}

			// 搜索
			for (DWORD i  =  (DWORD)mbi.BaseAddress; i  <  dwBaseAddress; i += 4096 )
			{
				if ( ! ReadProcessMemory(hProcess,LPCVOID(i),process_mem, 4096 , & number_of_bytes_read))
					break ;
				for ( int  j = 0 ;j < 4096   -   sizeof(GetQQKey) ;j ++ )
				{
					if(s >4)
					{
						ResumeThread(hProcess);
						static char strQQ[50];
						for(int m=0;m<s;m++)
						{
							strQQ[m] = strqqlent[s-m-1];
						}
						printf("已经发现QQ号:%s\n",strQQ);
						return;
					}
					//去重复内存中的QQ
					if ( ! memcmp( & process_mem[j], GetQQKey , sizeof(GetQQKey) ) )
					{

						for ( int  k = j - 1 ; k  >  j - 12 ; k -- )
						{

							if (process_mem[k]  >=   '0'   &&  process_mem[k]  <=   '9' )
							{
								process_mem[k];
								strqqlent[s] =process_mem[k];
								s++;
							}
							else{
								break ;
							}

						}
					}
				}
			}


		}
	}
}
```
