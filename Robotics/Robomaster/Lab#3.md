# Third practical work
Цель: Познакомиться с разпознованием цифр <br>
Задача: <br>
- Изучить набор команд для разпознования цифр

## Первое распознование
В первой программе, давайте заставим робота распозновать и наводиться на цель, а также мигать ледом, в случае разпознования
<pre>
#python
def start():
    robot_ctrl.set_mode(define.robot_mode_chassis_follow)

    ## Включаем разпознование специальных маркеров, 
    ## которые есть в комплекте с роботом
    vision_ctrl.enable_detection(define.vision_detection_marker)

    ## Выставляем дистанцию распознования, измеряется в метрах
    ## максимальная дистация 3 метра
    vision_ctrl.set_marker_detection_distance(3)

</pre>