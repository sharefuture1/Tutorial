# 常见部署方式

## 蓝绿部署

1. Blue/Green Deployment, 蓝绿部署的目的是减少发布的中断时间、能够快速撤回发布。
2. 蓝绿部署中一共有两套系统：一套是正在提供服务的系统，标记为“绿色”；另一套是准备发布的系统，标记为“蓝色”。两套系统都是功能完善的，并且正在运行的系统，只是系统版本和对外服务不同。
3. 蓝色系统经过反复的测试、修改、验证之后成功上线，直接将用户切换到蓝色系统。之前的绿色系统依旧运行，只是不让用户访问到，如果出现问题，直接切换到绿色系统。

`思考` 1. 蓝绿部署需要两套环境，费用是需要考虑的，并且维护两套环境，人力成本也要考虑。 2. 如果蓝色系统和绿色系统共同使用同一个数据库，那么这就意味着在切换蓝色和绿色系统的时候不会涉及到数据的丢失，但是如果本次上线涉及到旧表的修改，则可能会造成蓝色系统部分功能不能正常工作。如果蓝色系统和绿色系统不共同使用同一个数据库，那么在切换蓝色和绿色系统的时候要核对系统的数据，避免在切换的间隙造成数据丢失。 3. 蓝色系统和绿色系统的切换最好选在夜深人静的时候切换。

## 金丝雀发布

1. Canary Deployment, 和灰度发布同属一种策略，是一种平滑过渡的发布策略。
2. 金丝雀部署是只有一套系统，逐步替换的方式。
3. 金丝雀发布的几个步骤
   * 准备阶段，脚本、包、配置文件等等
   * 从ELB中将“金丝雀”移除，在AWS中称为Outofservice
   * 升级“金丝雀”应用
   * 对应用进行测试
   * 将“金丝雀”服务器重新添加到ELB中
   * 测试通过后依次升级其他服务器

     注释：矿井中的金丝雀 17世纪，英国矿井工人发现，金丝雀对瓦斯这种气体十分敏感。空气中哪怕有极其微量的瓦斯，金丝雀也会停止歌唱；而当瓦斯含量超过一定限度时，虽然鲁钝的人类毫无察觉，金丝雀却早已毒发身亡。当时在采矿设备相对简陋的条件下，工人们每次下井都会带上一只金丝雀作为“瓦斯检测指标”，以便在危险状况下紧急撤离。

`思考` 1. 金丝雀发布后的测试可以通过配置host来访问站点测试 2. 金丝雀发布同样对一些表结构的修改有局限性

