# tkinter

## 窗口

### 1.建立

```python
tk = Tk（）#tk是自定义的对象名，也可以是其他的名称
tk.mainloop（）#放在程序的最后一行
```

### 2.窗口的相关属性

1. title（）   可以设置窗口标题

2. geometry（“width x height + x + y”）窗口的宽和高，x， y是与屏幕的距离

3. maxsize（width， height） 拖曳时可以设置的窗口最大值的宽和高

4. minsize（width， height）拖曳时可以设置的窗口最小值的宽和高

5. configure（bg = “color”）窗口的背景颜色

6. resizable（Ture， Ture）是否更改窗口大小，第一个是宽，后面一个是高，固定的话改为（0， 0）

7. state（“zoomed”）最大化

8. iconify（）最小化

9. iconbitmap（“xx.ico”）窗口的图标

### 3.widget部件

1. Button按钮

2. Canvas画布

3. Checkbutton多选按钮

4. Entry文本框

5. Frame框架

6. Label标签

7. LabelFrame标签框架

8. Listbox列表框

9. Menu菜单

10. MenuButton菜单按钮

11. Message消息

12. OptionMenu下拉式菜单

13. PanedWindow面板

14. Radiobutton单选按钮

15. Scale尺度

16. Scrollbar滚动条

17. Spinbox可微调输入控件

18. Text文字区域

19. Toplevel上层窗口

### 4.widget共性

1. Dimension大小

2. color颜色

3. font字形

4. anchor锚

5. relief style属性边框

6. bitmap位图

7. cursor鼠标外形


