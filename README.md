# Day25project

실습 # 1 
Day006  에 마우스 좌클릭시 전체 화면 지우기

Day006  에 마우스 우클릭시 타이머 중지 on/off




// Main.cpp : 응용 프로그램에 대한 진입점을 정의합니다.
//

#include "stdafx.h"
#include "Main.h"
#include <time.h>
#define MAX_LOADSTRING 100
void CALLBACK TimerProc(HWND hWnd, UINT uMsg, UINT idEvent, DWORD dwTime);

// 전역 변수:
HINSTANCE hInst;                                // 현재 인스턴스입니다.
WCHAR szTitle[MAX_LOADSTRING];                  // 제목 표시줄 텍스트입니다.
WCHAR szWindowClass[MAX_LOADSTRING];            // 기본 창 클래스 이름입니다.

// 이 코드 모듈에 포함된 함수의 선언을 전달합니다:
ATOM                MyRegisterClass(HINSTANCE hInstance);
BOOL                InitInstance(HINSTANCE, int);
LRESULT CALLBACK    WndProc(HWND, UINT, WPARAM, LPARAM);
INT_PTR CALLBACK    About(HWND, UINT, WPARAM, LPARAM);

int APIENTRY wWinMain(_In_ HINSTANCE hInstance,
                     _In_opt_ HINSTANCE hPrevInstance,
                     _In_ LPWSTR    lpCmdLine,
                     _In_ int       nCmdShow)
{
    UNREFERENCED_PARAMETER(hPrevInstance);
    UNREFERENCED_PARAMETER(lpCmdLine);

    // TODO: 여기에 코드를 입력합니다.

    // 전역 문자열을 초기화합니다.
    LoadStringW(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
    LoadStringW(hInstance, IDC_WINAPI, szWindowClass, MAX_LOADSTRING);
    MyRegisterClass(hInstance);

    // 응용 프로그램 초기화를 수행합니다:
    if (!InitInstance (hInstance, nCmdShow))
    {
        return FALSE;
    }

    HACCEL hAccelTable = LoadAccelerators(hInstance, MAKEINTRESOURCE(IDC_WINAPI));

    MSG msg;

    // 기본 메시지 루프입니다:
    while (GetMessage(&msg, nullptr, 0, 0))
    {
        if (!TranslateAccelerator(msg.hwnd, hAccelTable, &msg))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }
    }

    return (int) msg.wParam;
}



//
//  함수: MyRegisterClass()
//
//  용도: 창 클래스를 등록합니다.
//
ATOM MyRegisterClass(HINSTANCE hInstance)
{
    WNDCLASSEXW wcex;

    wcex.cbSize = sizeof(WNDCLASSEX);

    wcex.style          = CS_HREDRAW | CS_VREDRAW |CS_DBLCLKS ;
    wcex.lpfnWndProc    = WndProc;
    wcex.cbClsExtra     = 0;
    wcex.cbWndExtra     = 0;
    wcex.hInstance      = hInstance;
    wcex.hIcon          = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_WINAPI));
    wcex.hCursor        = LoadCursor(nullptr, IDC_ARROW);
    wcex.hbrBackground  = (HBRUSH)(COLOR_WINDOW+1);
    wcex.lpszMenuName   = MAKEINTRESOURCEW(IDC_WINAPI);
    wcex.lpszClassName  = szWindowClass;
    wcex.hIconSm        = LoadIcon(wcex.hInstance, MAKEINTRESOURCE(IDI_SMALL));

    return RegisterClassExW(&wcex);
}

//
//   함수: InitInstance(HINSTANCE, int)
//
//   용도: 인스턴스 핸들을 저장하고 주 창을 만듭니다.
//
//   주석:
//
//        이 함수를 통해 인스턴스 핸들을 전역 변수에 저장하고
//        주 프로그램 창을 만든 다음 표시합니다.
//
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // 인스턴스 핸들을 전역 변수에 저장합니다.

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
	   CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}

//
//  함수: WndProc(HWND, UINT, WPARAM, LPARAM)
//
//  용도: 주 창의 메시지를 처리합니다.
//
//  WM_COMMAND  - 응용 프로그램 메뉴를 처리합니다.
//  WM_PAINT    - 주 창을 그립니다.
//  WM_DESTROY  - 종료 메시지를 게시하고 반환합니다.
//
//




LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
	PAINTSTRUCT ps;
	HDC hdc;


	/*static int x;
	static int y;
	static BOOL bnowDraw = FALSE;*/
	

	time_t mytime;
	static HANDLE hTimer, hTimer2;
	static const wchar_t * str;

	switch (message)
    {
		case WM_CREATE:
			SetTimer(hWnd, 1, 10, (TIMERPROC)TimerProc);
			break;
			//KillTimer(hWnd, 1); 메시지를 죽이는 문 ( 1에 들어가는 것은 이벤트ID)

		//case WM_CREATE:
		//	// 두번째 인자가 이벤트 ID
		//	hTimer = (HANDLE)SetTimer(hWnd, 1, 1000, NULL);//1초
		//	//Handle형으로 강제 캐스팅
		//	hTimer2 = (HANDLE)SetTimer(hWnd, 2, 5000, NULL);//5초
		//	str = L"";//비어있는 문자열 

		//	SendMessage(hWnd, WM_TIMER, 1, 0);

		//	break;
		//case WM_TIMER:
		////	//wParam으로 이벤트 ID가 전달이 된다.
		//switch (wParam)
		//{
		//		case 1:

		//			time(&mytime);
		//			str = _wctime(&mytime);//시간을 문자열로 변환 
		//			InvalidateRect(hWnd, NULL, TRUE);

		//			break;
		//		case 2:
		//			//MessageBeep(MB_OK);//단순 5초에 한번씩 소리를 내기 위해서 쓴 문장


		//			break;


		//			////}
		//			//if (wParam  == 1)
		//			//{
		//			//	time(&mytime);
		//			//	str = _wctime(&mytime);//시간을 문자열로 변환 
		//			//	InvalidateRect(hWnd, NULL, TRUE);
		//			//}
		//	}
		//	break;
    case WM_COMMAND:
        {
            int wmId = LOWORD(wParam);
            // 메뉴 선택을 구문 분석합니다:
            switch (wmId)
            {
            case IDM_ABOUT:
				// 모달 다이얼로그 와 모달리스 다이얼로그의 차이
                DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
                break;
            case IDM_EXIT:
                DestroyWindow(hWnd);
                break;
            default:
                return DefWindowProc(hWnd, message, wParam, lParam);
            }
        }
        break;
	//case WM_LBUTTONDOWN:
	//	x = LOWORD(lParam);
	//	y = HIWORD(lParam);
	//	bnowDraw = TRUE;

	//	break;
	//case WM_MOUSEMOVE://누른상태에서 true가 걸리니까 if 문으로 들어간다.
	//	if (bnowDraw == TRUE)//lParam에 x , y좌표가 다 들어와서 loword와 HIword로 좌표를 분리한다.
	//	{
	//		hdc = GetDC(hWnd);
	//		MoveToEx(hdc, x, y ,NULL);
	//		x = LOWORD(lParam);
	//		y = HIWORD(lParam);
	//		LineTo(hdc, x, y);
	//		ReleaseDC(hWnd, hdc);

	//	}
	//	break;
	//case WM_LBUTTONUP:
	//	bnowDraw = FALSE;

	//	break;
	case WM_LBUTTONDOWN: 


		InvalidateRect(hWnd, NULL, TRUE);
		SetTimer(hWnd, 1, 10, (TIMERPROC)TimerProc);
		return 0;

		break;
	case WM_RBUTTONDOWN:
		KillTimer(hWnd, 1);
		//SetTimer(hWnd, 1, 1000000000000, (TIMERPROC)TimerProc);
		//InvalidateRect(hWnd, NULL, FALSE);
		

		break;

    case WM_PAINT:
        {
            // TODO: 여기에 hdc를 사용하는 그리기 코드를 추가합니다...
			hdc = BeginPaint(hWnd, &ps);
			//TextOut(hdc, 100, 100, str, wcslen(str) - 1);






            EndPaint(hWnd, &ps);
        }
        break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// 정보 대화 상자의 메시지 처리기입니다.
INT_PTR CALLBACK About(HWND hDlg, UINT message, WPARAM wParam, LPARAM lParam)
{

    UNREFERENCED_PARAMETER(lParam);
    switch (message)
    {
    case WM_INITDIALOG:
        return (INT_PTR)TRUE;

    case WM_COMMAND:
        if (LOWORD(wParam) == IDOK || LOWORD(wParam) == IDCANCEL)
        {
            EndDialog(hDlg, LOWORD(wParam));
            return (INT_PTR)TRUE;
        }
        break;
    }
    return (INT_PTR)FALSE;
}
//typedef VOID(&TIMERPROC)(HWND,UINT,UINT,DWORD);

void CALLBACK TimerProc(HWND hWnd,UINT uMsg, UINT idEvent,DWORD dwTime)
{
	HDC hdc;
	int i;
	hdc = GetDC(hWnd);

	for (i = 0; i < 100; i++)
	{
		SetPixel(hdc, rand() % 500, rand() % 400,
			RGB(rand() % 256, rand() % 256, rand() % 256));//숫자는 0~ 255 까지 있다.
	}
	ReleaseDC(hWnd, hdc);


}
/*
# 실습 1.

*/

/*
# 숙제1

마우스 좌버튼 이벤트를 감지해서 클릭지점마다 직선으로 그려지게 구현하라
 - 최초 클릭시 지점이동
 - 3번째클릭시 최초=>2번 클릭지점까지 직선 
 - 2번째클릭시 2~3번 지점으로 직선

 # 숙제 2
 라면 타이머 만들기
 - 정중앙에 무슨 라면인지 출력
 - 우클릭 시작
 - 좌클릭 중지
 - 타이머 시간되면 메세지 박스
  


*/
