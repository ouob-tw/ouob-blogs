## 問題：新版edge瀏覽器無法指定數體大小數值，只有固定的選項


明明基於chormium的瀏覽器都可以修改指定大小
![[截圖 2025-07-15 11.47.39.png]]
就你edge不給改，還很貼心的把舊有設定放到自定裡，但無法修改啊😡




1. 關閉Edge瀏覽器，避免我們修改後的數值被覆蓋

2. 打開設定檔
```sh
nano ${HOME}/Library/Application Support/Microsoft Edge/Default/Preferences
```

3. 搜尋 minimum_font_size
	你應該可以找到以下json

	```
	{"webprefs":{"default_fixed_font_size":15,"default_font_size":18,"minimum_font_size":18}}
	```
4. 重新打開Edge瀏覽器