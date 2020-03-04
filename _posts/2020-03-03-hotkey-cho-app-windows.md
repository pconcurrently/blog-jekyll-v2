---
layout: post
title:  "Hotkey chuyển app nhanh cho người dùng Windows "
author: TrungNM
# categories: [ tutorial ]
featured: true
# hidden: true
tags: [mbi, trungnm]
---

## Chuẩn bị
Tải và cài [AutoHotKey](https://www.autohotkey.com/)

## Bắt đầu
Tạo 1 file `hotkey.ahk`

```autohotkey
RunOrActivate(Target, WinTitle = "")
{
	; Get the filename without a path
	SplitPath, Target, TargetNameOnly

	Process, Exist, %TargetNameOnly%
	If ErrorLevel > 0
		PID = %ErrorLevel%
	Else
		Run, %Target%, , , PID

	; At least one app (Seapine TestTrack wouldn't always become the active
	; window after using Run), so we always force a window activate.
	; Activate by title if given, otherwise use PID.
	If WinTitle <> 
	{
		SetTitleMatchMode, 2
		WinWait, %WinTitle%, , 3
		WinActivate, %WinTitle%
	}
	Else
	{
		WinWait, ahk_pid %PID%, , 3
		WinActivate, ahk_pid %PID%
	}
}


; Open App
>!c::
    RunOrActivate("C:\Program Files (x86)\Google\Chrome\Application\chrome.exe", "Chrome")
return

; Map to Mac
!c::Send, ^c 
!v::Send, ^v
!x::Send, ^x
;!z::Send, ^z Vietkey
!w::Send, ^w
!s::Send, ^s
!a::Send, ^a

; Other functions
!+f::WinMaximize, A
```

## Cách sử dụng
- <kbd>Ctrl</kbd> <kbd>Alt</kbd> <kbd>C</kbd>: sẽ chuyển sang Chrome ( hoặc bật nếu chưa có sẵn )
- <kbd>Alt</kbd> <kbd>C</kbd>: cho giống MacOs

Mình chỉ cung cấp công cụ thôi, autohotkey còn nhiều các lệnh khách, các bạn tự đọc thêm Docs và Google nếu muốn tùy chỉnh thêm

Happy Coding!
