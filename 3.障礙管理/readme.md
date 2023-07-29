#include "init.h"

//(19,0,0,5,5)
//

int ya = 0, yb = 1, cmin, cmax;

void setup() {
  FeInit();
  getDis();
  delayMicroseconds(250);
  static int ldis, fix5 = 0, lastColor = 0, turnback = 0;
  /*
     啟動 攝影機模組、光源感應器模組(tcs334725)、陀螺儀感應器模組(mpu6050)、伺服馬達模組。
     Activate the camera module, light sensor module (tcs334725), gyroscope sensor module (mpu6050), and servo motor module.
  */
  while (true) {
    rygSet(0, 0, 0);
    if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
      caturn_turn();
    } else {
      setTurn(getRelGyro(0) * 1.2);
    }
    motorSpd(200);
    /*
       朝向相對方位0度前進並在途中進行避障
       Move forward towards the relative direction of 0 degrees and perform obstacle avoidance along the way.
    */
    int color  = getTcs();
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
    /*
       利用光源感應器判斷行駛方向為順時針或逆時針
       Using the light sensor to determine the driving direction as clockwise or counterclockwise.
    */
  }
}
//20,220
void loop() {
  rygSet(0, 0, 0);
  static uint32_t a = 0, timer = 0, back = 0, straightTime = 0;
  motorSpd(250);
  for (int r = 0; r < 8; r++) {
    while (getTcs() < cmin || getTcs() > cmax) {
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      } else {
        setTurn(getRelGyro(a) * 1.2);
      }
    }
    /*
      朝向相對方位0度前進並在途中進行避障
      Move forward towards the relative direction of 0 degrees and perform obstacle avoidance along the way.
    */
    //************************************************************************************************
    a += ya * 90;
    /*
       利用車輛行駛方向判斷相對角度為順時針尋轉或逆時針旋轉
       Using the vehicle's driving direction to determine the relative angle as either a clockwise turn or a counterclockwise turn.
    */
    //*****************************************************************************
    while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      } else {
        setTurn(getRelGyro(a) * 1.2);
      }
    }
    /*
       車輛轉彎直到陀螺儀與相對目標偏差值小於10
       The vehicle turns until the gyroscope's relative deviation from the target is less than 10.
    */
  }//for
  //*********************************************************************************************
  //*************************************************************************************************
  if (ya == 1) {
    cmin = 200;
    cmax = 240;
  } else {
    cmin = 0;
    cmax = 40;
  }
  /*
     利用規則中迴轉更改光源感應器判斷目標
     Modify the target detection using the turning rules specified in the regulations, based on the light sensor.
  */
  straightTime = millis() + 200;
  while (millis() < straightTime) {
    motorSpd(0);
  }
  straightTime = millis() + 2000;
  while (millis() < straightTime) {
    motorSpd(-250);
    setTurn(getRelGyro(a) * (-1));
  }
  a += ya * 90;
  while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
    motorSpd(250);
    setTurn(getRelGyro(a));
  }
  /*
     車輛迴轉
     Vehicle turns back.
  */
  //****************************************************************************
  straightTime = millis() + 1000;
  while (millis() < straightTime) {
    setTurn(getRelGyro(a) * 1.2 + fix5);
    caturn_turn();
  }
  /*
       朝向相對方位0度前進並在途中進行避障
       Move forward towards the relative direction of 0 degrees and perform obstacle avoidance along the way.
  */
  //****************************************************************************
  for (int r = 0; r < (3 + (lastColor > 0)); r++) {
    while (getTcs() < cmin || getTcs() > cmax) {
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      } else {
        setTurn(getRelGyro(a) * 1.2);
      }
    }
    /*
      朝向相對方位0度前進並在途中進行避障
      Move forward towards the relative direction of 0 degrees and perform obstacle avoidance along the way.
    */
    //******************************************************************************************
    a += ya * 90 * lastColor;
    /*
       利用車輛行駛方向判斷相對角度為順時針尋轉或逆時針旋轉
       Using the vehicle's driving direction to determine the relative angle as either a clockwise turn or a counterclockwise turn.
    */
    //******************************************************************************************
    while (getRelGyro(a) > 10 || getRelGyro(a) < -10) {
      if ((CameraGet(3) > 15) || (CameraGet(7) > 15)) {
        caturn_turn();
      } else {
        setTurn(getRelGyro(a) * 1.2);
      }
    }
    /*
       車輛轉彎直到陀螺儀與相對目標偏差值小於10
       The vehicle turns until the gyroscope's relative deviation from the target is less than 10.
    */
  }
  //*******************************************************************************************
  straightTime = millis() + 800;
  while (millis() < straightTime) {
    motorSpd(250);
    caturn_turn();
  }
  while (1) {
    motorSpd(0);
  }
  /*
     車輛停止回起點區
     The vehicle stops and returns to the starting point area
  */
}
