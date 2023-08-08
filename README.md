# College-entrance-examination-countdown-card
College entrance examination countdown card

源代码:
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include<time.h>
#include<graphics.h>

#pragma comment( linker, "/subsystem:windows /entry:mainCRTStartup" )

using namespace std;

int zX = 305;
int zY = 205;
int T = 900000;
IMAGE background;

int standard_to_stamp(char* str_time)
{
	struct tm stm;
	int iY, iM, iD, iH, iMin, iS;
	memset(&stm, 0, sizeof(stm));
	iY = atoi(str_time);
	iM = atoi(str_time + 5);
	iD = atoi(str_time + 8);
	iH = atoi(str_time + 11);
	iMin = atoi(str_time + 14);
	iS = atoi(str_time + 17);
	stm.tm_year = iY - 1900;
	stm.tm_mon = iM - 1;
	stm.tm_mday = iD;
	stm.tm_hour = iH;
	stm.tm_min = iMin;
	stm.tm_sec = iS;
	return (int)mktime(&stm);
}

int main()
{
	loadimage(&background, _T("BG.png"),290,175);
	int pX = 1625;//班级1625
	int pY = -32;//班级-30
	time_t now_time = time(NULL);
	struct tm t_tm;
	localtime_s(&t_tm, &now_time);
	int start_time = (int)mktime(&t_tm);
	int end_time = standard_to_stamp("2024-06-07 0:0:0");
	int days = (end_time - start_time) / 24.0 / 60 / 60;
	initgraph(zX, zY,EW_NOCLOSE);
	HWND hWnd = GetHWnd();
	SetWindowPos(hWnd, HWND_TOPMOST, pX, pY, zX, zY, SWP_SHOWWINDOW);
	SetWindowText(hWnd, "高考倒计时 -by 屈圣桥");
	putimage(0,-10, &background);

	settextcolor(YELLOW);
	if ((days + 1) <= 100) settextcolor(RED);
	if (((days + 1) % 10) == 0) settextcolor(RED);
	LOGFONT f;
	gettextstyle(&f);						// 获取当前字体设置
	f.lfHeight = 88;						// 设置字体高度为 48
	f.lfWidth = 50;
	_tcscpy(f.lfFaceName, _T("黑体"));		
	f.lfQuality = ANTIALIASED_QUALITY;		// 设置输出效果为抗锯齿  
	settextstyle(&f);						// 设置字体样式


	TCHAR shengyutianshu[20];
	_stprintf_s(shengyutianshu, _T(" %d "), days+1);
	outtextxy(3, 60, shengyutianshu);
	
	Sleep(2000);
	while (1)
	{
		ExMessage msmsg;
		msmsg = getmessage(EM_MOUSE | EM_KEY);
		switch (msmsg.message)
			case WM_LBUTTONDOWN:
		{
			srand((unsigned int)time(NULL));
			int tmp = rand()%2;
			if(tmp==0)
			{ 
				for (int i = 0; i < 40; i += 1)
				{
					int y = 0;
					y = 1 * i * i;
					SetWindowPos(hWnd, HWND_TOPMOST, pX, pY + y, zX, zY, SWP_SHOWWINDOW);
					Sleep(10);
					flushmessage();
				}
				SetWindowPos(hWnd, HWND_TOPMOST, pX, pY - 400, zX, zY, SWP_SHOWWINDOW);
				pY -= 400;
				Sleep(T);
				for (int i = 0; i < 400; i++)
				{
					SetWindowPos(hWnd, HWND_TOPMOST, pX, pY + i, zX, zY, SWP_SHOWWINDOW);
					flushmessage();
					Sleep(1);
				}
				pY += 400;
			}
			else if(tmp==1)
			{
				for (int i = 0; i < 400; i += 4)
				{
					SetWindowPos(hWnd, HWND_TOPMOST, pX + i, pY, zX, zY, SWP_SHOWWINDOW);
					flushmessage();
					Sleep(10);
				}
				SetWindowPos(hWnd, HWND_TOPMOST, pX+400, pY , zX, zY, SWP_SHOWWINDOW);
				pX += 400;
				Sleep(T);
				for (int i = 0; i < 400; i++)
				{
					SetWindowPos(hWnd, HWND_TOPMOST, pX-i, pY, zX, zY, SWP_SHOWWINDOW);
					flushmessage();
					Sleep(1);
				}
				pX -= 400;
			}
               
		}
	}

	//cout << "距离高考还有" << days << "天" << endl;
	system("pause");
	return 0;
}
