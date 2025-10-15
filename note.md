#include <windows.h>

// 窗口过程函数（处理窗口消息）
LRESULT CALLBACK WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)
{
    switch (msg)
    {
    case WM_DESTROY:
        PostQuitMessage(0); // 退出消息循环
        return 0;
    }
    return DefWindowProc(hwnd, msg, wParam, lParam);
}

// WinMain 程序入口（Win32 应用程序）
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance,
                   LPSTR lpCmdLine, int nCmdShow)
{
    // 1. 定义并注册窗口类
    const wchar_t CLASS_NAME[] = L"MyWindowClass";

    WNDCLASS wc = {};
    wc.lpfnWndProc   = WndProc;       // 窗口过程
    wc.hInstance     = hInstance;     // 当前应用程序实例
    wc.lpszClassName = CLASS_NAME;    // 窗口类名
    wc.hCursor       = LoadCursor(NULL, IDC_ARROW); // 鼠标样式
    wc.hbrBackground = (HBRUSH)(COLOR_WINDOW+1);    // 背景色

    RegisterClass(&wc);

    // 2. 创建窗口
    HWND hwnd = CreateWindowEx(
        0,                      // 窗口扩展样式
        CLASS_NAME,             // 窗口类名
        L"Win32 Window Example",// 窗口标题
        WS_OVERLAPPEDWINDOW,    // 窗口风格
        CW_USEDEFAULT, CW_USEDEFAULT, 800, 600, // 窗口位置和大小
        NULL,       // 父窗口
        NULL,       // 菜单
        hInstance,  // 实例句柄
        NULL        // 附加参数
    );

    if (hwnd == NULL)
    {
        return 0;
    }

    ShowWindow(hwnd, nCmdShow);

    // 3. 消息循环
    MSG msg = {};
    while (GetMessage(&msg, NULL, 0, 0))
    {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }

    return 0;
}
