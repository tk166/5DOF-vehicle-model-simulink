# 5DOF-vehicle-model-simulink
An vehicle modle of simulink that can be used for HIL test or driving simulators in real-time


## 输入信息：
1. sta  输入的是EPS系统下端转向器小齿轮的转角，单位为deg，除以转向传动比即为前轮的转角大小。
2. thr_m 手动驾驶模式下的油门踏板开度，取值范围为0~1.
3. brk_m 手动驾驶模式下的制动踏板开度，取值范围为0~1.
4. mu_road 为一个4*1大小的向量，输入的是FL、FR、RL、RR四个轮胎与地面接触时的摩擦系数，这些值可以根据需求实时发生变化。
5. spd_target ①自动驾驶模式下为输入给车辆自带ACC控制器的期望车速，单位为km/h。
②当进行车辆状态初始化的时候，该值为初始化车辆状态后的车辆初始速度大小。 其中车辆状态的初始化发生在1)模型开始运行的时刻, 或者2)通过reset信号手动初始化车辆状态的时刻。
6. acc_mode 控制开启或者关闭车辆自带ACC控制器的开关（基于PID算法实现），当该值为0时，车辆ACC关闭为手动驾驶模式；当该值为1的时候ACC开启自动跟随期望车速。
7. reset 车辆初始化控制。当输入一个上升沿信号的瞬间进行车辆状态初始化。
## 输出信息：
1. Info 总线为一个Simulink Bus变量，包含车辆中的各种状态参数信息。
Info总线的具体成员（均采用右手坐标系，例如侧向速度向左为正）：
1. drv_sta 实际输入给车辆模型的转向器小齿轮的转角，单位为deg。
2. drv_thr 实际输入给车辆模型的油门踏板开度。取值范围0~1。当处于手动驾驶模式时，该值为驾驶员手动输入的油门踏板开度；当处于自动驾驶模式时，该值为ACC控制器输出的油门踏板开度。
3. drv_brk 实际输入给车辆模型的制动踏板开度。取值范围0~1。当处于手动驾驶模式时，该值为驾驶员手动输入的制动踏板开度；当处于自动驾驶模式时，该值为ACC控制器输出的制动踏板开度。
4. str_Pwheel 为1*2大小的向量，其中的值分别为FL、FR两个轮胎的转角大小，单位为deg。
5. str_Mpinion 转向器小齿轮处的转向力矩大小（驾驶员通过转向盘实际感受到的力矩来自这里），单位N-m。
6. pwr_weng 发动机的曲轴转速，单位为rpm。
7. pwr_wgrout 变速箱输出轴的转速，单位为rpm。
8. pwr_stc 液力变矩器输出轴与输入轴转速的比值大小，影响力传递的效率。
9. pwr_Te 发动机输出的力矩，单位为N-m。
10. pwr_Ttcin 液力变矩器的输入力矩大小，单位为N-m。
11. pwr_Ttcout 液力变矩器的输出力矩大小，单位为N-m。
12. pwr_Tdrv 为1*4大小的向量，其中的值分别为FL、FR、RL、RR驱动轴上的驱动力矩大小，单位为N-m。由于是前置前驱车辆，其中RL、RR上的驱动力矩始终为零。
13. brk_Tb  为1*4大小的向量，其中的值分别为FL、FR、RL、RR轮毂上的制动力矩大小，单位为N-m。
14. veh_Ax 车身坐标系下车辆的纵向加速度大小，单位为 m/s^2。
15. veh_Vx 车身坐标系下车辆的纵向速度大小，单位为 km/h。
16. veh_PX 大地坐标系下车辆位置的X坐标，单位为m。
17. veh_Ay 车身坐标系下车辆的侧向加速度大小，单位为 m/s^2。
18. veh_Vy 车身坐标系下车辆的侧向速度大小，单位为 km/h。
19. veh_Py 大地坐标系下车辆位置的Y坐标，单位为m。
20. veh_Vyaw 车辆的横摆角速度大小，单位为deg/s。
21. veh_Pyaw 车辆的横摆角大小，单位为deg。
22. veh_Vroll 车辆的侧倾角速度大小，单位为deg/s。
23. veh_Proll 车辆的侧倾角大小，单位为deg。
24. tir_Fx 为大小4*1的向量，其中的值为FL、FR、RL、RR轮胎受到的纵向力大小，单位N。
25. tir_Fy 为大小4*1的向量，其中的值为FL、FR、RL、RR轮胎受到的侧向力大小，单位N。
26. tir_Fz 为大小4*1的向量，其中的值为FL、FR、RL、RR轮胎受到的垂向力大小，单位N。
27. tir_Mz 为大小4*1的向量，其中的值为FL、FR、RL、RR轮胎受到的回正力大小，单位N-m。
28. tir_Mzsum FL、FR轮胎回正力之和，单位N-m。注意由于轮边动力学的存在，FL、FR轮胎回正力之和并不等于转向器小齿轮的转向力大小。
29. tir_Vwheel 为大小4*1的向量，其中的值为FL、FR、RL、RR轮胎的转速，单位为rad/s。
30. tir_aw 为大小4*1的向量，其中的值为FL、FR、RL、RR轮胎的轮胎侧偏角，单位为deg。
31. tir_kw 为大小4*1的向量，其中的值为FL、FR、RL、RR轮胎的实际纵向滑移率，无量纲。
32. tir_kw0 为大小4*1的向量，其中的值为假定FL、FR、RL、RR轮胎是刚性的时的纵向滑移率，无量纲。
33. tir_mu 为一个4*1大小的向量，其中的值为FL、FR、RL、RR四个轮胎与地面接触时的摩擦系数，无量纲。
34. tir_chw 由于轮边几何参数和当前轮胎位置共同造成的车轮外倾角大小，单位为deg。
