---
layout: post
title: 强大的Make
---
每一次用Make都会不禁感叹这个东西有多么的强大，强烈建议所有做开发工作，也推荐不做开发工作，但是有许多各种各样文件需要处理的人深入学习一下Make，当然，简单学习一下其实挺简单的，能做的事也很简单就是了，要强大当然还是要深入的学习。

下面是我的一个模板，比较方便，共享出来给懒人们玩玩。基本上如果是写个简单的项目，这个就满足绝大多数需求了，甚至包含了对头文件依赖的处理，确实非常方便。

{% highlight make %}
CC=clang
CXX=clang++

TARGET=test
C_SRCS=$(wildcard *.c)
C_OBJS=${C_SRCS:%.c=%.o}

CXX_SRCS=$(wildcard *.cpp)
CXX_OBJS=${CXX_SRCS:%.cpp=%.o}

$(TARGET) : .c_depend .cxx_depend $(C_OBJS) $(CXX_OBJS)
	$(CXX) $(LDFLAGS) $(C_OBJS) $(CXX_OBJS) -o $@

.c_depend : $(C_SRCS)
	$(CC) $(CFLAGS) -MM $(C_SRCS) > .c_depend
	
.cxx_depend : $(CXX_SRCS)
	$(CXX) $(CXXFLAGS) -MM $(CXX_SRCS) > .cxx_depend

%.o : %.c
	$(CC) $(CFLAGS) -c $< -o $@

%.o : %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

-include .c_depend .cxx_depend

clean :
	rm *.o $(TARGET) .c_depend .cxx_depend
{% endhighlight %}
