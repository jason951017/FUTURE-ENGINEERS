# 機器人比賽工程日誌 - 視覺感應避障電動車

## 簡介

歡迎來到我們的機器人比賽工程日誌！我們準備參加一場視覺感應避障電動車的比賽。我們的目標是製作一台自動導航並能根據視覺感應器來避免障礙物的電動車。這份日誌將記錄我們在組裝和優化車輛過程中的進展、挑戰和解決方案。
# 壹.移動式管理

## 1. 馬達選擇

在初期階段，我們仔細考慮了不同的馬達選項，最終選擇了更小的N20馬達，而不是EV3樂高馬達。這個選擇不僅讓我們的車輛更加緊湊，還能提供更好的靈活性，以實現有效的閃避障礙物。

![N20馬達](1.移動性管理/n20馬達.jpeg)
![EV3馬達](1.移動性管理/EV3馬達.jpg)

## 2. 底盤設計

為了確保車輛的機動性，我們決定使用3D列印技術設計一個自定義的底盤，同時解決了遙控車底盤迴轉半徑過大的問題。這個決定將讓我們製作一個專為我們需求度身訂造的底盤，以提高整體性能。

### 遙控車迴轉半徑：

![遙控車](1.移動性管理/搖1.jpg)
![遙控車](1.移動性管理/搖2.jpg)

### 3D列印車迴轉半徑：

![3D車](1.移動性管理/自1.jpg)
![3D車](1.移動性管理/自2.jpg)

由上圖比對發現遙控車底盤迴轉半徑過大，可能影響閃避交通號誌，所以最終選擇用3D列印底盤車解決此問題。

## 3. 3D列印齒輪

為了確保馬達和樂高零件的齒輪相匹配，我們自行使用3D列印技術設計了合適的齒輪。這將實現在車輛內部有效地傳遞動力。

![自製齒輪](1.移動性管理/齒輪.jpg)

## 4. 馬達轉速

我們進行了馬達轉速的測試，以瞭解其性能特點。以下是馬達在不同電壓下的轉速結果圖：

紅色框選部分為我們挑選馬達的數據

![馬達轉速圖](1.移動性管理/馬達轉速.png)

## 5. 差速器齒輪比

為了實現差速操控，我們計算並設計了適合後輪的差速器齒輪比大約等於400*(16-24)=266.667

![差速器齒輪](1.移動性管理/差速器.jpg)

## 6. 裝配說明

以下是包括底盤和框架的裝配指南和零件實裝圖：

![裝配說明](1.移動性管理/底盤解說圖.png)
![](1.移動性管理/底盤裝配圖.jpg)

## 7. 3D元件三視圖及stl圖檔

整個車輛元件的三視圖：

### 頂視圖：

![頂視圖](1.移動性管理/視圖.png)

### 正視圖：

![正視圖](1.移動性管理/上視圖.png)

### 側視圖：

![側視圖](1.移動性管理/側視圖.png)

### stl圖檔：

[stl圖檔](1.移動性管理/FE全.stl)
