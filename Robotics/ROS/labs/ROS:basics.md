<h1>Лабораторная работа на тему ROS:basics</h1>
Задача данной лабораторной работы состоит в том, чтобы запустить симуляцию, которая будет запускать черепашку. <br>
<pre>
Key words:
</pre>
<br>
turtle sim:
<code>$ rosrun turtlesim turtlesim_node</code>

Turtlebot:
<code>$ export TURTLEBOT3_MODEL=${TB3_MODEL}</code>
<code>$ roslaunch turtlebot3_bringup turtlebot3_robot.launch</code>
<code>$ export TURTLEBOT3_MODEL=burger</code>
<code>$ roslaunch turtlebot3_slam turtlebot3_slam.launch</code>
<code>$ export TURTLEBOT3_MODEL=burger</code>
<code>$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch</code>


