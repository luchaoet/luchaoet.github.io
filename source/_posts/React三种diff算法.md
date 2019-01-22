---
title: React三种diff算法
date: 2018-10-09 16:50:50
tags: React
summary: diff作为 Virtual DOM 的加速器，其算法上的改进是React整个界面渲染的基础和性能保障
---

<span data-type="color" style="color:rgb(36, 41, 46)"><span data-type="background" style="background-color:rgb(255, 255, 255)">diff作为 Virtual DOM 的加速器，其算法上的改进是React整个界面渲染的基础和性能保障</span></span>
<span data-type="color" style="color:rgb(36, 41, 46)"><span data-type="background" style="background-color:rgb(255, 255, 255)">React将虚拟DOM树转化为真实DOM树的最少操作的过程称为</span></span><span data-type="color" style="color:rgb(36, 41, 46)"><span data-type="background" style="background-color:rgb(255, 255, 255)"><strong>调和</strong></span></span><span data-type="color" style="color:rgb(36, 41, 46)"><span data-type="background" style="background-color:rgb(255, 255, 255)">（reconciliation diff算法便是调和过程的具体实现）</span></span>
diff算法有三个策略：
1. web UI中DOM节点跨层级的移动操作特别少 可以忽略不计
2. 拥有相同类的两个组件将会生成相似的树形结构 拥有不同类的组件将会生成不同的树形结构
3. 同一层级的一组子节点 它们会通过唯一的id进行区别
<span data-type="color" style="background:rgba(0, 0, 0, 0.5)"><span data-type="background" style="background-color:rgb(255, 255, 255)">基于以上策略 React分别对tree diff，component diff,element diff进行算法优化，事实证明这3个前提策略是合理且准确的，它保证了整个界面构建的性能</span></span>

# 1. tree diff
React对树的算法进行了简洁明了的优化，即对树进行分层比较，两棵树只会对同一层次的节点进行比较
特点：
* 删除节点及其子节点
* 一个节点一个节点的依次创建
* 并不会移动节点树

React通过updateDepth对Virtual DOM树进行层级控制，只会对相同层级的DOM节点进行比较，即同一父节点下的所有子节点，当发现子节点已经不存在时，则该节点及其子节点被完全删除，不会用于比较
这样只需要对树进行一次遍历便完成整个DOM树的比较

如果出现DOM节点跨层级的移动操作，如图



![屏幕快照 2018-10-09 下午5.33.56.png | center | 400x277.4049217002237](https://cdn.nlark.com/yuque/0/2018/png/115449/1539077671035-ee21f261-0e3f-4875-897f-e6624429532d.png "")

A节点及其子节点整个被移动到了D节点下，由于React只会简单的考虑同层级节点的位置变换，而对于不同层级的节点，只会创建和删除
当根节点发现子节点A消失了，就会直接销毁A
当D发现多了一个子节点A，则会创建A（及其子节点）作为其子节点

此时diff的执行请情况是：
create A -> create B -> create C -> delete A

由此当出现节点跨层级移动时，并不会出现移动操作，而是以A节点的整个树被创建，这是一种影响性能的操作，因此官方不建议进行DOM节点跨层级操作，在组件开发中 保持稳定的DOM结构会有助于性能的提升，例如可以通过css的隐藏或显示节点 而不是真正的移除或添加DOM节点

# 2. component diff
React是基于组件构建的应用，对于组件间的比较所采用的策略也是非常的简洁高效的
* 如果是同一类型的组件，按照原策略继续比较Virtual DOM树
* 如果不是同一类型的组件，则将该组件判断为dirty component 从而替代整个组件下的所有子组件
* 对于同一类型的组件，有可能其Virtual DOM没有任何变化，如果能够确切知道这点，那么就可以节省大量的diff运算时间。因此React允许用户通过shouldComponentUpdate()来判断该组件是否需要进行diff算法分析


![屏幕快照 2018-10-09 下午5.56.09.png | center | 600x245.839874411303](https://cdn.nlark.com/yuque/0/2018/png/115449/1539079012691-4892ca8b-48fa-4d67-a5a2-90dcf07c1ecd.png "")

当组件D变为组件G时，即使这两个组件结构相似，一旦React判断D和G是不用类型的组件，就不会比较两者的结构，而是直接删除组件D，重新创建组件G及其子节点。虽然当两个组件是不同类型但结构相似时，进行diff算法分析会影响性能，但是毕竟不同类型的组件存在相似DOM树的情况在实际开发过程中很少出现，因此这种极端因素很难在实际开发过程中造成重大影响

# 3. element diff
当节点处于同一层级时，diff提供3种节点操作，分别是 
* IMSERT\_MARKUP(插入)
* MOVE\_EXISTING(移动)
* REMOVE\_NODE(删除)

* IMSERT\_MARKUP——新的组件类型不在旧集合里，即全新的节点，需要对新节点执行插入操作
* MOVE\_EXISTING——旧集合中有新组件类型，且element是可更新的类型 generateComponentChildren 已调用receiveComponent 这种情况下 precChild = nextChid就需要做移动操作，可以复用以前的DOM节点
* REMOVE\_NODE——旧组件类型，在新集合里也有，但对应的element不同则不能直接复用和更新，需要执行删除操作，或者旧组件不在新集合里的，也需要执行删除操作
# 

