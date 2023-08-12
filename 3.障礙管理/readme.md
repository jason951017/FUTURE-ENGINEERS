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
