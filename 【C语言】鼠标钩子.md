#include<stdio.h>
#include<windows.h>
HHOOK handle = 0;

LRESULT CALLBACK hooks(HHOOK nCode, WPARAM wParam, LPARAM lParam)
{
	MSLLHOOKSTRUCT *jiegou = { 0 };
	jiegou = (MSLLHOOKSTRUCT*)lParam;
	printf("x坐标：%d   y坐标：%d   操作：", jiegou->pt.x, jiegou->pt.y);
	if (wParam == 512)printf("移动\n");
	if (wParam == 522)printf("滚轮滚动\n");
	if (wParam == 513)printf("左键按下\n");
	if (wParam == 514)printf("左键弹起\n");
	if (wParam == 516)printf("右键按下\n");
	if (wParam == 517)printf("右键弹起\n");
	return CallNextHookEx(handle, nCode, wParam, lParam);
}

int main()
{
	handle = SetWindowsHookExA(WH_MOUSE_LL, hooks, NULL, NULL);
	if (handle == NULL)
	{
		return 0;
	}
	SetConsoleTitle("鼠标Hook     您可以按下F12来卸载钩子");
	MSG xiaoxi;
	SHORT vk;
	while (1)
	{
		PeekMessage(&xiaoxi, NULL, 0, 0, PM_NOREMOVE);
		TranslateMessage(&xiaoxi);
		DispatchMessage(&xiaoxi);
		vk = GetAsyncKeyState(123);
		if (vk != NULL && handle != NULL)
		{
			UnhookWindowsHookEx(handle);
			handle = NULL;
			system("cls");
			system("color 0c");
			printf("钩子已卸载\n");
		}
	}
}
