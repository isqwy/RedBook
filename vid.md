# VID方言

VID是(**V**isual **I**nterface **D**ialect)的缩写。
是在`View`引擎之上提供的一套描述GUI的简洁方言。
VID允许对每个待显示的图形组件指定不同的布局方法：
*  水平或垂直的流式布局；
*  网格定位；
*  绝对定位；

VID自动对 `face` 描述提供一个容器 `face`，默认类型为 `window`

VID代码被 `layout`函数处理，以生成 face树。(该函数被 view 函数内调。)

在控制台输入 `help view` 或 `help layout` 可查看相关帮助信息。