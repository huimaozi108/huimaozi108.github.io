---
layout: post
title: Ubuntu 14.04下Sublime Text 3的安装，使用搜狗中文输入的解决办法
category: 技术
tags: [Ubuntu Sublime]
keywords: Ubuntu,14.04,Sublime,搜狗输入法,中文输入
description: 
---

>最近试着在Ubuntu编程，我安装了Ubuntu 14.04.1 X64版本，并安装了我最喜欢的Sublime Text 3编辑器，可是却无法输入中文，在网上找遍资料，最终虽然能够输入中文了，但是还不完美（输入框无法跟随光标）。

##截图预览

![预览](/post_res/2014-10-10/img/ubuntu_sublime_im.png)

##步骤

###第一步、安装搜狗输入法

- 首先到[搜狗输入法 Linux版](http://pinyin.sogou.com/linux/?r=pinyin){:target="_blank"}下载安装包
- 如果Ubuntu是14.04，则直接安装，12.04的话按照官网的教程安装
- 安装后重启机器就可以使用搜狗输入法了，但是在Sublime Text 3中还无法使用

###第二步、给Sublime打补丁

1. 保存下面的代码为`sublime_imfix.c`

```c
/*
sublime-imfix.c
Use LD_PRELOAD to interpose some function to fix sublime input method support for linux.
By Cjacker Huang <jianzhong.huang at i-soft.com.cn>
 
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
LD_PRELOAD=./libsublime-imfix.so sublime_text
*/
#include <gtk/gtk.h>
#include <gdk/gdkx.h>
typedef GdkSegment GdkRegionBox;
 
struct _GdkRegion
{
  long size;
  long numRects;
  GdkRegionBox *rects;
  GdkRegionBox extents;
};
 
GtkIMContext *local_context;
 
void
gdk_region_get_clipbox (const GdkRegion *region,
            GdkRectangle    *rectangle)
{
  g_return_if_fail (region != NULL);
  g_return_if_fail (rectangle != NULL);
 
  rectangle->x = region->extents.x1;
  rectangle->y = region->extents.y1;
  rectangle->width = region->extents.x2 - region->extents.x1;
  rectangle->height = region->extents.y2 - region->extents.y1;
  GdkRectangle rect;
  rect.x = rectangle->x;
  rect.y = rectangle->y;
  rect.width = 0;
  rect.height = rectangle->height; 
  //The caret width is 2; 
  //Maybe sometimes we will make a mistake, but for most of the time, it should be the caret.
  if(rectangle->width == 2 && GTK_IS_IM_CONTEXT(local_context)) {
        gtk_im_context_set_cursor_location(local_context, rectangle);
  }
}
 
//this is needed, for example, if you input something in file dialog and return back the edit area
//context will lost, so here we set it again.
 
static GdkFilterReturn event_filter (GdkXEvent *xevent, GdkEvent *event, gpointer im_context)
{
    XEvent *xev = (XEvent *)xevent;
    if(xev->type == KeyRelease && GTK_IS_IM_CONTEXT(im_context)) {
       GdkWindow * win = g_object_get_data(G_OBJECT(im_context),"window");
       if(GDK_IS_WINDOW(win))
         gtk_im_context_set_client_window(im_context, win);
    }
    return GDK_FILTER_CONTINUE;
}
 
void gtk_im_context_set_client_window (GtkIMContext *context,
          GdkWindow    *window)
{
  GtkIMContextClass *klass;
  g_return_if_fail (GTK_IS_IM_CONTEXT (context));
  klass = GTK_IM_CONTEXT_GET_CLASS (context);
  if (klass->set_client_window)
    klass->set_client_window (context, window);
 
  if(!GDK_IS_WINDOW (window))
    return;
  g_object_set_data(G_OBJECT(context),"window",window);
  int width = gdk_window_get_width(window);
  int height = gdk_window_get_height(window);
  if(width != 0 && height !=0) {
    gtk_im_context_focus_in(context);
    local_context = context;
  }
  gdk_window_add_filter (window, event_filter, context); 
}
```

2. 安装C/C++的编译环境和`gtk libgtk2.0-dev`

```
sudo apt-get install build-essential
sudo apt-get install libgtk2.0-dev
```

3. 编译共享内库

```
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
```

4. 将共享库拷贝到Sublime Text 3目录下(默认在/opt/sublime_text下)

```
sudo cp libsublime-imfix.so /opt/sublime_text/
```

5. 编辑/usr/bin/subl文件

```
#!/bin/sh
SUBLIME_HOME="/opt/sublime_text"
LD_LIB=$SUBLIME_HOME/libsublime-imfix.so
Exec= bash -c "LD_PRELOAD=$LD_LIB $SUBLIME_HOME/sublime_text" "$@"
```

6. 编辑/usr/share/applications/sublime_text.desktop,将里面的`/opt/sublime_text/sublime_text`全部替换为‘subl’


##参考
1. [Ubuntu系统下Sublime Text 2中fcitx中文输入法的解决方法](http://forum.ubuntu.org.cn/viewtopic.php?t=418712)
2. [完美解决 Linux 下 Sublime Text 中文输入](http://my.oschina.net/tsl0922/blog/113495)
3. [Ubuntu下Sublime Text 3解决无法输入中文的方法](http://jingyan.baidu.com/article/f3ad7d0ff8731609c3345b3b.html)