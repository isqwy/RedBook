# 值型转换

关键字`as`用于值型转换：
```
as <new-type> value
as [<new-type>] value                  ;-- alternative syntax

<new-type> : integer! | byte! |  logic! | c-string! | float! | float32! |
             pointer! [integer!] | struct! [<members>] |
             <alias-name>
```

示例：

```
foo: 0                                 ;-- foo is an integer variable
bar: declare pointer! [integer!]       ;-- bar is a pointer variable

foo: as integer! bar                   ;-- type casting
bar: as pointer! [integer!] foo
```

## 转换表

<div align="center">
<table class="matrix">
<tr>

<th id="matrix-start">source&gt;&gt;</th>
<th>byte!</th>
<th>integer!</th>
<th>logic!</th>
<th>c-string!</th>
<th>pointer!</th>
<th>struct!</th>
<th>float!</th>
<th>float32!</th>
<th>function!</th>
</tr><tr>

<th>byte!</th>
<td class="warn">WARNING</td>
<td class="allow">as byte! &sup1;</td>
<td class="allow">true<span class="arrow">&raquo;</span>#"^(01)"<br>false<span class="arrow">&raquo;</span>#"^(00)"</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
</tr><tr>

<th>integer!</th>
<td class="allow">as integer!</td>
<td class="warn">WARNING</td>
<td class="allow">true<span class="arrow">&raquo;</span>1<br>false<span class="arrow">&raquo;</span>0</td>
<td class="allow">as integer!</td>
<td class="allow">as integer!</td>
<td class="allow">as integer!</td>
<td class="deny">ERROR</td>
<td class="allow">as integer!</td>
<td class="allow">as integer!</td>
</tr><tr>

<th>logic!</th>
<td class="allow">#"^(00)"<span class="arrow">&raquo;</span>false<br>else<span class="arrow">&raquo;</span>true</td>
<td class="allow">0<span class="arrow">&raquo;</span>false<br>else<span class="arrow">&raquo;</span>true</td>
<td class="warn">WARNING</td>
<td class="allow">null<span class="arrow">&raquo;</span>false<br>else<span class="arrow">&raquo;</span>true</td>
<td class="allow">null<span class="arrow">&raquo;</span>false<br>else<span class="arrow">&raquo;</span>true</td>
<td class="allow">null<span class="arrow">&raquo;</span>false<br>else<span class="arrow">&raquo;</span>true</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
</tr><tr>

<th>c-string!</th>
<td class="deny">ERROR</td>
<td class="allow">as c-string!</td>
<td class="deny">ERROR</td>
<td class="warn">WARNING</td>
<td class="allow">as c-string!</td>
<td class="allow">as c-string!</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
</tr><tr>

<th>pointer!</th>
<td class="deny">ERROR</td>
<td class="allow">as pointer!</td>
<td class="deny">ERROR</td>
<td class="allow">as pointer!</td>
<td class="warn">WARNING</td>
<td class="allow">as pointer!</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
</tr><tr>

<th>struct!</th>
<td class="deny">ERROR</td>
<td class="allow">as struct!</td>
<td class="deny">ERROR</td>
<td class="allow">as struct!</td>
<td class="allow">as struct!</td>
<td class="warn">WARNING</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
</tr>

<th>float!</th>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="warn">WARNING</td>
<td class="allow">to float!</td>
<td class="deny">ERROR</td>
</tr>

<th>float32!</th>
<td class="deny">ERROR</td>
<td class="allow">as float32!</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="allow">to float32!</td>
<td class="warn">WARNING</td>
<td class="deny">ERROR</td>
</tr>

<th>function!</th>
<td class="deny">ERROR</td>
<td class="allow">as function!</td>
<td class="deny">ERROR</td>
<td class="allow">as function!</td>
<td class="allow">as function!</td>
<td class="allow">as function!</td>
<td class="deny">ERROR</td>
<td class="deny">ERROR</td>
<td class="allow">as function!</td>
</tr>

</table>
</div>
