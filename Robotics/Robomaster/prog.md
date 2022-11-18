<pre>
Example of LED blinking: 

def start():
    
    for i in range(10):
        led_ctrl.set_flash(rm_define.armor_all,i+1)
        
        led_ctrl.set_top_led(rm_define.armor_top_all,255,0,0,rm_define.effect_flash)
        led_ctrl.set_bottom_led(rm_define.armor_bottom_all,255,255,0,rm_define.effect_flash)
        
        time.sleep(1)
</pre>
<br>
<pre>
Example of chassis moving:

def start():
    
    robot_ctrl.set_mode(rm_define.robot_mode_free)
    
    for i in range(2):
        chassis_ctrl.set_wheel_speed(50,-50,50,-50)
        
        time.sleep(1)
        
        chassis_ctrl.set_wheel_speed(-50,50,-50,50)
        
        time.sleep(1)
</pre>
<br>
