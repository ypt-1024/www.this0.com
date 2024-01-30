---
title: collision
tags:
  - Godot
categories:
  - Godot
copyright_author: godot
copyright_author_href: 'https://space.bilibili.com/479412933'
copyright_url: 'https://github.com/zfoo-project/godot-start'
copyright_info: 此文章版权归godot所有，如有转载，请注明来自原作者
abbrlink: 5bcd23be
date: 2024-01-30 13:39:20
description:
---

# 1. 物理系统

- 物理系统（PhysicsSystem）用于管理所有物理相关的功能，目前它负责同步物理元素、触发物理事件和调度物理世界的迭代。
- 物理世界迭代时会对物理元素进行物理计算，比如计算各物体是否产生碰撞，以及物体的受力情况。当计算完成后，物理系统会将物理世界更新到场景世界中，从而使游戏对象产生相应的物理行为。
- Godot 的物理系统提供了高效的组件化工作流程和便捷的使用方法。目前支持刚体、碰撞组件、触发和碰撞事件
- 用一句话总结物理系统，就是模拟真实世界的碰撞

![Image text](http://cdn.this0.com/blog/img/collision-1.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000001&Signature=RbJhSHs3Vgho/kAhIsF7mCqzeYY=&x-oss-process=style/cdn.this0)
![Image text](http://cdn.this0.com/blog/img/collision-2.png)
![Image text](http://cdn.this0.com/blog/img/collision-3.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000000&Signature=FQNSIXfX6PpsY9EFp6ad9D4lBJc=&x-oss-process=style/cdn.this0)
![Image text](http://cdn.this0.com/blog/img/collision-4.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000001&Signature=LvvrY82bj2DSDBJTUMXfmJGAIY8=&x-oss-process=style/cdn.this0)
![Image text](http://cdn.this0.com/blog/img/collision-5.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000001&Signature=B3R2bkTrS5wcrNEV/1dPDCaPG8s=&x-oss-process=style/cdn.this0)
![Image text](http://cdn.this0.com/blog/img/collision-6.png)

# 2. RigidBody节点

- 刚体是组成物理世界的基本对象，你可以将刚体想象成一个你不能看到（绘制）也不能摸到（碰撞）的带有属性的物体。

![Image text](http://cdn.this0.com/blog/img/collision-8.png?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000000&Signature=lot9KDk0OZDKel0ZMntqksN2/eg=&x-oss-process=style/cdn.this0)

- mass，刚体质量
- weight，刚体加速度
- linear velocity，移动速度
- angular velocity，旋转速度
- applied forces，施加的力
- torque，扭矩
- damp，衰减系数，阻尼，值越大物体移动越慢，可以用来模拟空气摩擦力等效果。

# 3. 刚体实战

- RigidBody，动态刚体，有质量，可以设置速度，会受到重力影响。

- Kinematic，运动刚体，零质量，可以设置速度，不会受到重力的影响，但是可以设置速度来进行移动。

- Static，静态刚体，零质量，零速度，即不会受到重力或速度影响，但是可以设置他的位置来进行移动

- Area2D，一块区域，能够检测到物体的碰撞，但是不会做任何操作

- 碰撞检测Layer和Mask，Layer表明当前刚体属于那一层，Mask表面当前物体的遮罩

![Image text](http://cdn.this0.com/blog/img/collision-9.JPG)
![Image text](http://cdn.this0.com/blog/img/collision-10.JPG?OSSAccessKeyId=LTAI5tAje5MhbPSKCC6QdGZb&Expires=9000000000&Signature=r/8iX5x15e29iD231txogA8EXqo=&x-oss-process=style/cdn.this0)
