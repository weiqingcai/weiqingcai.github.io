---
title: Android自动化测试
date: 2018-11-09 16:49:37
tags: [Android,自动化测试]
categories: Android
---
# Android自动化测试

>1. adb shell monkey -p package [事件数]

用于对指定包名的应用进行压力测试

>2. adb logcat | find "标签字符串"

用于将logcat中的命令输出到控制台

>3. adb shell monkey --throttle [milliseconds]

每步操作之间间隔时长为 milliseconds

>4. adb shell monkey -s seed [event-count]

指定一个随机生成的值，每一个随机值确定一组相同的随机操作

>5. adb shell monkey --ptc-touch [percent]

设定一组monkey事件组中触摸事件所占百分比

>6. adb shell monkey --ptc-motion [percent]

设定一组monkey事件组中动作事件所占百分比

>7. adb shell monkey --ptc-trackball [percent]

设定一组monkey事件组中轨迹球事件所占百分比

>8. adb shell monkey --ptc-nav [percent]

设定一组monkey事件组中基本导航事件所占百分比

>9. adb shell monkey --ptc-majornav [percent]

设定一组monkey事件组中主要导航事件所占百分比

>10. adb shell monkey --ptc-syskeys [percent]

设定一组monkey事件组中系统导航事件所占百分比

>11. adb shell monkey --ptc-appswitch [percent]

设定一组monkey事件组中启动Activity事件所占百分比

>12. adb shell monkey --ptc-anyevent [percent]

设定一组monkey事件组中不常用事件所占百分比

>13. adb shell monkey --ignore-crashs [event-count]

忽略崩溃事件继续进行测试

>13. adb shell monkey --ignore-timeout [event-count]

忽略ANR事件继续进行测试

>14. adb shell monkey -f [scriptfile] [event-count] 

执行monkey脚本
