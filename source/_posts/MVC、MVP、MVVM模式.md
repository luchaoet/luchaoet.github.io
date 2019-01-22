---
title: MVC/MVP/MVVM模式
date: 2018-10-22 15:28:11
tags: ['MVC','MVP','MVVM']
summary: 前端开发中的MVC/MVP/MVVM三种模式
---
MVC，MVP和MVVM都是常见的软件架构设计模式（Architectural Pattern），它通过分离关注点来改进代码的组织方式。不同于设计模式（Design Pattern），只是为了解决一类问题而总结出的抽象方法，一种架构模式往往使用了多种设计模式。

要了解MVC、MVP和MVVM，就要知道它们的相同点和不同点。不同部分是C(Controller)、P(Presenter)、VM(View-Model)，而相同的部分则是MV(Model-View)。

## MVC


![屏幕快照 2018-10-22 下午5.26.44.png | center | 747x393](https://cdn.nlark.com/yuque/0/2018/png/115449/1540200427279-4b524054-64b0-4ef1-8905-acc17eee33a8.png "")

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">MVC允许在不改变视图的情况下改变视图对用户输入的响应方式，用户对View的操作交给了Controller处理，在Controller中响应View的事件调用Model的接口对数据进行操作，一旦Model发生变化便通知相关视图进行更新。</span></span>
### <span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Model</span></span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Model层用来存储业务的数据，一旦数据发生变化，模型将通知有关的视图。</span></span>
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">Model和View之间使用了观察者模式，View事先在此Model上注册，进而观察Model，以便更新在Model上发生改变的数据。</span></span>
### View
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">view和controller之间使用了策略模式，这里View引入了Controller的实例来实现特定的响应策略</span></span>
### Controller
控制器是模型和视图之间的纽带，MVC将响应机制封装在controller对象中，当用户和你的应用产生交互时，控制器中的事件触发器就开始工作了。

MVC模式的业务逻辑主要集中在Controller，而前端的View其实已经具备了独立处理用户事件的能力，当每个事件都流经Controller时，这层会变得__十分臃肿__。而且MVC中View和Controller一般是一一对应的，捆绑起来表示一个组件，视图与控制器间的过于紧密的连接让Controller的复用性成了问题。

## MVP


![屏幕快照 2018-10-22 下午5.49.43.png | center | 747x417](https://cdn.nlark.com/yuque/0/2018/png/115449/1540201798824-8b13a984-2926-4d15-a28b-5575d2b6bdf0.png "")

<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">MVP（Model-View-Presenter）是MVC模式的改良，由IBM的子公司Taligent提出。和MVC的相同之处在于：Controller/Presenter负责业务逻辑，Model管理数据，View负责显示。</span></span>
虽然在MVC里，View是可以直接访问Model的，但MVP中的View并不能直接使用Model，而是通过为Presenter提供接口，让Presenter去更新Model，再通过观察者模式更新View。
与MVC相比，MVP模式通过解耦View和Model，完全分离视图和模型使职责划分更加清晰；由于View不依赖Model，可以将View抽离出来做成组件，它只需要提供一系列接口提供给上层操作。（组件开发）
### Model
Model层依然是主要与业务相关的数据和对应处理数据的方法。
### View
MVP定义了Presenter和View之间的接口，用户对View的操作都转移到了Presenter。比如这里可以让View暴露setter接口以便Presenter调用，待Presenter通知Model更新后，Presenter调用View提供的接口更新视图。
### Presenter
Presenter作为View和Model之间的“中间人”，除了基本的业务逻辑外，还有大量代码需要对从View到Model和从Model到View的数据进行“手动同步”，这样Presenter显得很__重__，维护起来会比较困难。而且由于没有数据绑定，如果Presenter对视图渲染的需求增多，它不得不过多关注特定的视图，一旦视图需求发生改变，Presenter也需要改动。

## MVVM
MVVM（Model-View-ViewModel）最早由微软提出。ViewModel指 "Model of View"——视图的模型
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">MVVM把View和Model的同步逻辑自动化了。以前Presenter负责的View和Model同步不再手动地进行操作，而是交给框架所提供的数据绑定功能进行负责，只需要告诉它View显示的数据对应的是Model哪一部分即可。</span></span>
### Model
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">在MVVM中，我们可以把Model称为数据层，因为它仅仅关注数据本身，不关心任何行为（格式化数据由View的负责），这里可以把它理解为一个类似json的数据对象。</span></span>
### View
<span data-type="color" style="color:rgb(51, 51, 51)"><span data-type="background" style="background-color:rgb(255, 255, 255)">和MVC/MVP不同的是，MVVM中的View通过使用模板语法来声明式的将数据渲染进DOM，当ViewModel对Model进行更新的时候，会通过数据绑定更新到View。</span></span>
### ViewModel
ViewModel大致上就是MVC的Controller和MVP的Presenter了，也是整个模式的重点，业务逻辑也主要集中在这里，其中的一大核心就是数据绑定，后面将会讲到。与MVP不同的是，没有了View为Presente提供的接口，之前由Presenter负责的View和Model之间的数据同步交给了ViewModel中的数据绑定进行处理，当Model发生变化，ViewModel就会自动更新；ViewModel变化，Model也会更新。

整体来看，比MVC/MVP精简了很多，不仅仅简化了业务与界面的依赖，还解决了数据频繁更新（以前用jQuery操作DOM很繁琐）的问题。因为在MVVM中，View不知道Model的存在，ViewModel和Model也察觉不到View，这种低耦合模式可以使开发过程更加容易，提高应用的可重用性。
