## First practical work
Цель: познакомиться с Robomaster EP <br>
Задачи: <br>
    - Понять какие комплектующие есть у комплекта Robomaster EP <br>
    - Разобрать python команды для Robomaster <br>
    - Написать первые программы для Robomaster <br>


## Комплектующие Robomaster EP
С комплектующими набора Robomaster EP можно познакомиться, посмотрев <a href='https://dl.djicdn.com/downloads/robomaster-s1/20191030/RoboMaster_S1_Quick_Start_Guide_v1.4_EN.pdf'>инструкцию Robomaster</a>. 

## Знакомство с средой Robomaster


## Hello world in terms of Robotics
Первая программа позволит просто подвинуть робота вперед на несколько метров<br>
Команда для того, чтобы робот поехал вперед: <br>
<pre>
#python
def start():
    ## далеее будет команда специально для Robomaster
    ## заставляет робота поворачиваться за подвеской (деталь: Gimbal)
    robot_ctrl.set_mode(rm_define.robot_mode.free)
    ## далее устанавливается скорость вращения для каждого колеса
    ## контроль_шасси.установить_скорость(переднее_левое, переднее_правое, заднее_левое, заднее_правое)
    ## задайти скорость 30 для первой программы
    ## за один раз, робот проезжает приблизительно 1 метр
    for i in range(1):
        chassis_ctrl.set_wheel_speed()
        time.sleep(1)
</pre>
Первая программа на Python для Robomaster написана.
<br>
 <br>
 Во второй программе давайте попробуем изменить цвет диодов диодами<br>
 <pre>
 def start():
    ## программа движения, которую вы ранее написали
    led_ctrl.set_top_led(rm_define.armor_top_all,
                255, 255, 255,rm_define.effect_always_off)
    led_ctrl.set_bottom_led(rm_define.armor_bottom_all,
                255, 255, 255,rm_define.effect_always_on)
 </pre><br>
 <br>
 Третья программа заставит робота разворачиваться и поъезжать обратно.
 <pre>
 def start():
    chassis_ctrl.rotate_with_degree(rm_define.clockwise, 90)
    time.sleep(1)
 </pre>
<br>

## Техническое задание 
1) Создать программу, благодаря которой робот будет объезжать периметр квадрата (1мx1м), задайте цвет диодов (любой цвет).
2) Создать программу, благодаря которой робот будет объезжать периметр звезды, задайте цвет диодов
3) ** Создайте программу, которая способна выполнять действия из первых двух программ по хлопку (ссылка на репозиторию, в которой можно посмотреть пример кода: https://github.com/ROBOMASTER-S1/ROBOMASTER-S1-Python-Examples/blob/master/Clap%20Scheme%20Examples.py) 