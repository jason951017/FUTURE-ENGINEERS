# 機器人比賽工程日誌 - 視覺感應避障電動車

## 簡介

歡迎來到我們的機器人比賽工程日誌！我們準備參加一場視覺感應避障電動車的比賽。我們的目標是製作一台自動導航並能根據視覺感應器來避免障礙物的電動車。這份日誌將記錄我們在組裝和優化車輛過程中的進展、挑戰和解決方案。
# 壹.移動性管理

## 一. 馬達選擇

在初期階段，我們仔細考慮了不同的馬達選項，最終選擇了更小的N20馬達，而不是EV3樂高馬達。這個選擇不僅讓我們的車輛更加緊湊，還能提供更好的靈活性，以實現有效的閃避障礙物。

![N20馬達](1.移動性管理/n20馬達.jpeg)
![EV3馬達](1.移動性管理/EV3馬達.jpg)

## 二. 底盤設計

為了確保車輛的機動性，我們決定使用3D列印技術設計一個自定義的底盤，同時解決了遙控車底盤迴轉半徑過大的問題。這個決定將讓我們製作一個專為我們需求度身訂造的底盤，以提高整體性能。

### 遙控車迴轉半徑：

![遙控車](1.移動性管理/搖1.jpg)
![遙控車](1.移動性管理/搖2.jpg)

### 3D列印車迴轉半徑：

![3D車](1.移動性管理/自1.jpg)
![3D車](1.移動性管理/自2.jpg)

由上圖比對發現遙控車底盤迴轉半徑過大，可能影響閃避交通號誌，所以最終選擇用3D列印底盤車解決此問題。

## 三. 3D列印齒輪

為了確保馬達和樂高零件的齒輪相匹配，我們自行使用3D列印技術設計了合適的齒輪。這將實現在車輛內部有效地傳遞動力。

![自製齒輪](1.移動性管理/齒輪.jpg)

## 四. 馬達轉速

我們進行了馬達轉速的測試，以瞭解其性能特點。以下是馬達在不同電壓下的轉速結果圖：

紅色框選部分為我們挑選馬達的數據

![馬達轉速圖](1.移動性管理/馬達轉速.png)

## 五. 差速器齒輪比

為了實現差速操控，我們計算並設計了適合後輪的差速器齒輪比大約等於400*(16-24)=266.667

![差速器齒輪](1.移動性管理/差速器.jpg)

## 六. 裝配說明

以下是包括底盤和框架的裝配指南和零件實裝圖：

![裝配說明](1.移動性管理/底盤解說圖.png)
![](1.移動性管理/底盤裝配圖.jpg)

## 七. 3D元件三視圖及stl圖檔

整個車輛元件的三視圖：

### 頂視圖：

![頂視圖](1.移動性管理/視圖.png)

### 正視圖：

![正視圖](1.移動性管理/上視圖.png)

### 側視圖：

![側視圖](1.移動性管理/側視圖.png)

### stl圖檔：

[stl圖檔](1.移動性管理/FE全.stl)

# 貳.電源和感測器管理

## 一. nicjoy14500金屬鋰電池

這是車輛的動力來源，提供電力給主控板、馬達以及其他電子裝置。選用金屬鋰電池而不是模型電池是因為nicjoy金屬鋰電池體積更小，更方便縮小車輛體積及配重。
一節電池電壓為3.7 ~ 4.2伏特，穩態電壓3.7伏特，我們使用兩節電池實際電壓為7.4 ~ 8.4伏特，電壓輸入到主控版後會經過穩壓晶片將電壓穩壓到5伏特和3.3伏特再分配到主控板和其他感應器。

![image](2.電源和感測器管理/電池.jpeg)

## 二.主控板

Krduino主控板：這是整個車輛的主要控制板，負責管理和協調其他零件的運作。主控板通過程式碼控制各個感測器和執行器的動作，實現車輛的自主運行和障礙物避免功能。

![image](2.電源和感測器管理/Krduino.jpg)

  #### 二-1 馬達驅動馬達驅動版tb6612

   tb6612是用於控制電動車的馬達。在車輛中，tb6612驅動晶片負責控制馬達的速度和方向，進而實現車輛的運動和轉向控制。已內建整合於主控板中。

   ![image](2.電源和感測器管理/ttb6612.jpg)

## 三.陀螺儀mpu6050

這是一個慣性感測器，能夠檢測車輛的傾斜和轉動。通過i2c通訊讀取陀螺儀數據，主控板可以判斷車輛的傾斜狀態，從而實現平穩的移動和轉向。

![image](2.電源和感測器管理/mpu6050.jpg)

## 四.顏色感應器tcs34725

這個感應器可以識別周圍環境的顏色。在比賽中，我們通過i2c通訊讀取感應器數據來辨別地面狀態，以便車輛做出適當的反應和閃避動作。

![image](2.電源和感測器管理/TCS34725.jpg)

## 五.攝影機esp32cam

攝影機是視覺感應器的一種，它可以拍攝周圍的場景。透過攝影機的影像處理，車輛可以識別前方的障礙物或路徑，並進行適應性的運動控制。

![image](2.電源和感測器管理/esp32cam.jpg)

## 六.超音波hc-sr04

這個超音波感測器可以測量車輛與前方障礙物之間的距離。藉由這個數據，車輛可以避免與障礙物碰撞，實現安全的運行。

![image](2.電源和感測器管理/hc-sr04.jpg)

## 七.按鈕

按鈕是一種輸入裝置，可以用來控制車輛的啟動、停止或其他功能。在比賽中，我們將按鈕用來操控車輛的啟動。

![image](2.電源和感測器管理/按鈕.jpg)

## 八.三色指示燈

這些燈光裝置用來顯示車輛的狀態或提供提示信息。例如，可以利用指示燈顯示車輛是否處於運行中，或者是否偵測到了障礙物，若程式有誤還能方便及時更正程式。

![image](2.電源和感測器管理/燈.jpg)

# 參.障礙管理

以下是我們車輛的程式碼
```
#include "init.h"

int ya = 0, yb = 1, cmin, cmax;

void setup() {
  // 初始化感應器
  FeInit();
  getDis();
  delayMicroseconds(250);
  static int ldis, fix5 = 0, lastColor = 0, turnback = 0;
  while (true) {
    // 沿陀螺儀相對零度角前進並進行避障
    rygSet(0, 0, 0);
    if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
      caturn_turn();
      if (CameraGet(3) > 15) fix5 = -20;
      if (CameraGet(7) > 15) fix5 = 20;
    } else {
      setTurn(getRelGyro(0) * 1.2);
    }
    motorSpd(250);
    // 利用光源感應器分辨地面上藍橘線得知行進方向為順時針或逆時針
    int color = getTcs();
    if (color > 0 && color < 40) {
      ya = 1;
      cmin = 0;
      cmax = 40;
      break;
    }
    if (color > 200 && color < 240) {
      ya = -1;
      cmin = 200;
      cmax = 240;
      break;
    }
  }
}

// 20,220
void loop() {
  rygSet(0, 0, 0);
  static byte b = 0, c = 0;
  static int ldis, fix5 = 0, lastColor = 0;
  static uint32_t a = 0, timer = 0, back = 0, straightTime = 0;
  motorSpd(250);
  // 記數進行8次
  for (int r = 0; r < 8; r++) {
    // 前進直到偵測到地圖上藍橘線
    while (getTcs() < cmin || getTcs() > cmax) {
      // 沿陀螺儀相對零度角前進並進行避障
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      } else {
        setTurn(getRelGyro(a + fix5) * 1.2);
      }
      if (r == 4) {
        if (CameraGet(3) > 15) {
          // 將相對角目標調整90度
          a += ya * 90;
        }
        //*****************************************************************************
        // 轉向直到與目標角度相差小於10度
        while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
          motorSpd(250);
          // 在觀測到障礙後進行避障
          if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
            caturn_turn();
          }
          setTurn(getRelGyro(a) * 1.2);
        }
      }
    }
  }
  //*********************************************************************************************
  straightTime = millis() + 700;
  if (lastColor == -1) {
    rygSet(0, 0, 0);
    // 前進0.7秒並在途中進行避障
    while (millis() < straightTime) {
      motorSpd(250);
      caturn_turn();
    }
    //*************************************************************************************************
    // 改變地面上藍橘線目標色相值
    if (ya == 1) {
      cmin = 200;
      cmax = 240;
    } else {
      cmin = 0;
      cmax = 40;
    }
    // 沿相對角180度後退
    straightTime = millis() + 1800;
    while (millis() < straightTime) {
      motorSpd(-250);
      setTurn(getRelGyro(a) * (-1));
    }
    // 轉向直到與目標角相差小於10度
    a += ya * 90;
    while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
      motorSpd(250);
      setTurn(getRelGyro(a) * 1.2);
    }
  }
  //****************************************************************************
  // 前進1秒並在途中進行避障
  straightTime = millis() + 1000;
  while (millis() < straightTime) {
    setTurn(getRelGyro(a) * 1.2);
    caturn_turn();
  }
  //****************************************************************************
  // 記數3+(是否往反方向)次
  for (int r = 0; r < (3 + (lastColor > 0)); r++) {
    // 前進直到偵測到地圖上藍橘線
    while (getTcs() < cmin || getTcs() > cmax) {
      // 沿陀螺儀相對零度角前進並進行避障
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      } else {
        setTurn(getRelGyro(a + fix5) * 1.2);
      }
    }
    //******************************************************************************************
    // 將目標角調整90度
    a += ya * 90 * lastColor;
    //******************************************************************************************
    // 轉向直到與目標角度相差小於10度
    while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
      motorSpd(250);
      // 在觀測到障礙後進行避障
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      }
      setTurn(getRelGyro(a) * 1.2);
    }
  }
  //*******************************************************************************************
  // 前進0.8秒後停止
  straightTime = millis() + 800;
  while (millis() < straightTime) {
    motorSpd(250);
    caturn_turn();
  }
  while (1) {
    motorSpd(0);
  }
}

```
```
#include "init.h"

int ya = 0, yb = 1, cmin, cmax;

void setup() {
  // 初始化感應器
  FeInit();
  getDis();
  delayMicroseconds(250);
  static int ldis, fix5 = 0, lastColor = 0, turnback = 0;
  while (true) {
    // 沿陀螺儀相對零度角前進並進行避障
    rygSet(0, 0, 0);
    if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
      caturn_turn();
      if (CameraGet(3) > 15) fix5 = -20;
      if (CameraGet(7) > 15) fix5 = 20;
    } else {
      setTurn(getRelGyro(0) * 1.2);
    }
    motorSpd(250);
    // 利用光源感應器分辨地面上藍橘線得知行進方向為順時針或逆時針
    int color = getTcs();
    if (color > 0 && color < 40) {
      ya = 1;
      cmin = 0;
      cmax = 40;
      break;
    }
    if (color > 200 && color < 240) {
      ya = -1;
      cmin = 200;
      cmax = 240;
      break;
    }
  }
}

// 20,220
void loop() {
  rygSet(0, 0, 0);
  static byte b = 0, c = 0;
  static int ldis, fix5 = 0, lastColor = 0;
  static uint32_t a = 0, timer = 0, back = 0, straightTime = 0;
  motorSpd(250);
  // 記數進行8次
  for (int r = 0; r < 8; r++) {
    // 前進直到偵測到地圖上藍橘線
    while (getTcs() < cmin || getTcs() > cmax) {
      // 沿陀螺儀相對零度角前進並進行避障
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      } else {
        setTurn(getRelGyro(a + fix5) * 1.2);
      }
      if (r == 4) {
        if (CameraGet(3) > 15) {
          // 將相對角目標調整90度
          a += ya * 90;
        }
        //*****************************************************************************
        // 轉向直到與目標角度相差小於10度
        while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
          motorSpd(250);
          // 在觀測到障礙後進行避障
          if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
            caturn_turn();
          }
          setTurn(getRelGyro(a) * 1.2);
        }
      }
    }
  }
  //*********************************************************************************************
  straightTime = millis() + 700;
  if (lastColor == -1) {
    rygSet(0, 0, 0);
    // 前進0.7秒並在途中進行避障
    while (millis() < straightTime) {
      motorSpd(250);
      caturn_turn();
    }
    //*************************************************************************************************
    // 改變地面上藍橘線目標色相值
    if (ya == 1) {
      cmin = 200;
      cmax = 240;
    } else {
      cmin = 0;
      cmax = 40;
    }
    // 沿相對角180度後退
    straightTime = millis() + 1800;
    while (millis() < straightTime) {
      motorSpd(-250);
      setTurn(getRelGyro(a) * (-1));
    }
    // 轉向直到與目標角相差小於10度
    a += ya * 90;
    while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
      motorSpd(250);
      setTurn(getRelGyro(a) * 1.2);
    }
  }
  //****************************************************************************
  // 前進1秒並在途中進行避障
  straightTime = millis() + 1000;
  while (millis() < straightTime) {
    setTurn(getRelGyro(a) * 1.2);
    caturn_turn();
  }
  //****************************************************************************
  // 記數3+(是否往反方向)次
  for (int r = 0; r < (3 + (lastColor > 0)); r++) {
    // 前進直到偵測到地圖上藍橘線
    while (getTcs() < cmin || getTcs() > cmax) {
      // 沿陀螺儀相對零度角前進並進行避障
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      } else {
        setTurn(getRelGyro(a + fix5) * 1.2);
      }
    }
    //******************************************************************************************
    // 將目標角調整90度
    a += ya * 90 * lastColor;
    //******************************************************************************************
    // 轉向直到與目標角度相差小於10度
    while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
      motorSpd(250);
      // 在觀測到障礙後進行避障
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      }
      setTurn(getRelGyro(a) * 1.2);
    }
  }
  //*******************************************************************************************
  // 前進0.8秒後停止
  straightTime = millis() + 800;
  while (millis() < straightTime) {
    motorSpd(250);
    caturn_turn();
  }
  while (1) {
    motorSpd(0);
  }
}

```

# 肆.圖片-團隊以及汽車

1.團隊照片

![隊伍照片](隊伍照片1.jpg)
![隊伍照片](隊伍照片2.jpg)

2.車輛照片

![image](車1.jpg)

側視圖:

![image](側視車2.jpg)

上視圖:

![image](上視車1.jpg)
