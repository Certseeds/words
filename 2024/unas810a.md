---
author: "Certseeds"
date: "2024-06-02"
title: "U-NAS 810A Chassis customize hardware"
description: "万由810A机箱的硬件选择."
tags: ["hardware", "experience", "business"]
---

# 万由810A机箱的硬件选择

由于空间问题, 巨型全塔机箱满足不了需要, 选择了万由(U-NAS)的810A作为新NAS的机箱.

奇怪的英文参考链接: <https://www.u-nas.com/xcart/cart.php?target=product&product_id=17640>, 没找到中文版的链接.

## 购买方式

+ 不差钱可以考虑直接走上面的链接, 219.99$ (怎么是美元).
+ 不想折腾定制线走淘宝, 官方淘宝店带电源比不带贵400块左右.
+ 想折腾走闲鱼, 估计给下游的机箱和电源搭配比较自由, 便宜一些.

## 硬件选择考虑

万由810A, 宽33cm, 深30cm, 高28cm左右, 8个3.5英寸热插拔 SATA盘位, 可以放置MATX主板, 内置两个12cm大风扇和一个小风扇, 需要Flex/1U电源. 在配件选择上需要特别注意, 和大机箱不一样.

1. 定制电源线, 如果使用全模组的小1U电源的话得定制好几根线

+ 40cm的4pin+4pin CPU供电线只够直线从主板上穿过去, 无法从硬盘侧-右侧绕行, 75cm长度则完全足够, 有冗余, 可以考虑65cm.
+ 24cm的主板供电线只适合电源放在外面做测试用, 50cm则完全足够, 还有些长需要回绕一下, 为了机箱整洁可以考虑40cm.
+ 大4pin供电线不需要和SATA供电线混杂在一起, 机箱自带的硬盘背板只需要大4pin供电, 为了机箱整洁需要定制单独的大4pin线, 长度上20cm都有些长, 可以考虑10cm.

2. 风扇接口, 低价主板上风扇接口可能只有两个, 一个CPU, 另一位给其他机箱风扇, 这款需要一个一转三的转接线.

3. PCIE转接线不需要太长的, 只是转一个方向, 用普通款式即可.

4. PCIE直通卡使用的8087转SATA线注意不能和机箱出来的公口公♂对公♂的link, 得将机箱的背板拆下(四周的五颗螺丝, 左侧三颗螺丝, 右侧还有好几颗螺丝, 注意不需要拆风扇的粗螺丝), 取出原装的银色线, 将SATA公头装入机箱上的硬盘背板. 长度建议使用30cm+50cm组合, 30cm的给外侧4个SATA, 50cm给内侧4个SATA, 配合扎带可以提供出两条8087线给直通卡, 不需要再出来8个SATA了.

5. PCIE直通卡可以使用正常的全高款式, 就是风扇需要注意, 由于特殊的装配方式, 可能和其他线/PCIE转接线有交叠, 别打到.

## 安装注意事项

1. 最好有个长螺丝刀, 刀尖带点磁性最好, 省力.
2. 注意使用适合的十字刀头, 这些小螺丝看着不太坚固, 滑丝了就麻烦了.
3. 外壳需要先卸下五颗螺丝, 随后向前(还是后?)移动, 将固定的金属卡扣卸下, 向上, 向外抬起.
4. 热插拔硬盘支架和硬盘本体需要用小长盒里面的黑色螺丝固定在一起, 不然拔出来的时候硬盘就留里面了.
5. 机箱内一些孔洞旁如果有线, 建议用扎带扎好, 不会和风扇互动, 并且美观.
