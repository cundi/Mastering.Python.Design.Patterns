# Mastering.Python.Design.Patterns
译名《精通Python设计模式》

不同于上一本，学习设计模式，这本以Django框架来阐明多种设计模式。

开坑，有空，边翻边学。
第一章预览
*****
第一章 工厂模式
============
   创造模式处理一个对象的创建。创造模式的目的是为了在不按照约定而直接地创建的地方提供可选择的情况。  
   
  在工厂模式中，客户端查询一个对象而不知道这个对象来自哪里（即，哪一个类被用来生成它）。在一个工厂背后的思想是简化一个对象的创建。如果这个结果是通过一个中心函数来完成，相比之下要让一个客户端直接地使用类实例化来创建对象，跟踪哪一个对象被创建则会更容易些。通过分离要使用的代码，工厂减少了一个应用维护的复杂性。
    
工厂通常以两张形式出现：**工厂模式**，是一个每次输入参数都返回一个不同的方法（在Python术语中称为函数）；抽象工厂，它是一组工厂方法用于创建相关产品的系列。  

工厂方法
--------------
********
在工厂方法中，我们执行一个单独的函数，传递一个参数，它提供了我们想要的是*什么* 信息。我们不要求知道任何关于这个对象的*如何*实现的细节，以及它来自*哪里*。

### 一个真实的例子
一个工厂方法模式现实中的使用是在塑料玩具厂。
                       

抽象工厂
------------
******
抽象工厂设计模式是工厂模式的归纳。基本上，抽象工厂是一组(逻辑上的)工厂方法，这里每个工厂模式都负责生成一种不同的对象。

###一个真实的例子
抽象工厂用在车辆织造中。相同的机械装置用来冲压不同的车辆模型的部件（门，面板，车身罩体，挡泥板，以及各种镜子）。聚合了不同机械装置的模型是可配置的，而且在任何时候都可以轻松变更。在下图中我们可以看到一个车辆制造的抽象工厂的例子。   

###一个软件例子
**django_factory** 包是一个为了在测试中创建Django模型的抽象工厂实现。它用来创建支持指定测试属性的模型实例。这是很重要的因为测试变得可读，以及避免共享不必要的代码。
   
##使用案例
因为抽象工模式是工厂方法模式的归纳，它具有同样的优点：使得追踪一个对象创建更容易，它让对象的使用和创建分离开，并且给我们内存使用以及应用的性能提高的可能。   
  
但是问题来了：我们如何知道什么时候使用工厂模式还是抽象工厂？答案是我们通常以更简单的工厂模式开始。如果我们发现应用需要很多的工厂模式,要让它变得有意义就要合并同族对象，那么我们就使用它。抽象工厂的好处站在用户的角度来看不是很明显，但是在使用工厂模式时，通过改变活动的工厂方法它给了我们修改动态的应用（运行中的）的能力。经典的例子是给予改变一个应用的外观和感觉（例如，Apple以及Windows这样的系统，等等），对于用户来说就是在应用使用时，不需要终止它，启动它。   

##实现
  为了阐明抽象工厂模式，我将再次使用我最喜爱的例子之一。想象我们正在开发一款游戏，或者我们想把一款小游戏加入到自己的应用中来娱乐用户。我们想要至少包含两个游戏，一个给小孩子玩，一个给成人玩。基于用户输入，我们要决定在运用时创建哪一个游戏。一个抽象工厂便能做到游戏的创建部分。   
  
  让我们从儿童游戏开始吧。这个游戏叫做FrogWorld。主角是一个喜欢迟昆虫的青蛙。每个主角都需要的一个响亮的名号，在运行时，我们的这个例子中名字是由用户给出的。如下所示， interact_with()方法用于描述青蛙和障碍物（例如，虫子，迷宫以及别的青蛙）的互动：
    Class Frog:
		def __init__(self, name):
			self.name = name
			
		def __str__(self):
			return self.name
			
		def interact_with(self, obstacle):
			print('{} the Frog encounters {} and {}!'.format(self, obstacle, obstacle.action()))
  
  可以有很多种类的障碍，不过对于我们的这个例子来说可以只有一个Bug。青蛙遇到一个虫子，只有一个行为被支持：吃掉它！  
   
     Class Bug:
	 	def __str__(self):
			return 'a bug'
			   
		def action(self):
			return 'eats it'
  
  FrogWorld类是一个抽象工厂。它的主要责任是游戏的主角和障碍物。保持创建方法的独立和方法名称的普通（例如，make_character(), make _obstacle()）让我们可以动态的改变活动工厂（以及活动的游戏）而不设计任何的代码变更。如下所示，在一个静态类型语言中，抽象工厂可以是一个有着空方法的抽象类/接口，但是在Python中不要求这么做，因为类型在运行时才被检查。   
    Class FrogWorld:
		def __inti__(self, name):
			print(self)
			self.player_name = name
			
		def __str__(self):
			return '\n\n\t------ Frog World ------'
		
		def make_character(self):
			return Forg(self.player_name)
			
		def make_obstacle(self):
			return Bug()
			
   
   游戏WizarWorld也类似。唯一的不同是巫师不吃虫子而是与半兽人这样的怪物战斗！
     Class Wizard:
	 	def __init__(self, name):
			self.name = name
			
		def __str__(self):
			return self.name
			
		def interact_with(self, obstacle):
			print('{} the Wizard batteles against {} and {}!'.format(self, obstacle, obstacle.action()))
			
			
	Class Ork:
		def __str__(self):
			return 'an evial ork'
			
		def action(self):
			return 'kills it'
		
		
		
	Class WizardWorld:
		def __init__(self, name):
			print(self)
			self.player_name = name
			
		def __str__(self):
			return '\n\n\t------ Wizard World ------'
			
		def make_character(self):
			return Wizard(self.player_name)
			
		def make_obstacle(self):
			return Ork()
			
   
   GameEnviroment是我们游戏的主要入口。它接受factory作为输入，然后用它创建游戏的世界。play()方法发起创造的hero和obstacle之间的交互如下：  
   
    Class GameEnviroment:
		def __init__(self, factory):
			self.hero = factory.make_character()
			self.obstacle = factory.make_obstacle()
			
		def play(self):
			self.hero.interact_with(self.obstacle)
			
  validate_age()函数提示用户输入一个有效的年龄。若年龄无效，则返回一个第一个元素设置为False的元组。如下所示，若输入的年龄没问题，元组的第一个元素设置为True，以及我们所关心的该元组的第二个元素，即用户给定的年龄：
   
    def validate_age(name):
		try:
			age = input('Welcome {}. How old are you？'.format(name))
			age = int(age)
		except ValueError as err:
			print("Age {} is valid, plz try again...".format(age))
			return (False, age)
		return (True, age)
		
   
   最后，尤其是main()函数的出现。它询问用户的名字和年龄，然后由用户的年龄来决定哪一个游戏应该被运行：
     
	 def main():
	 	name = input("Hello. What's your name?")
		valid_input = False
		while not valid_input:
			valid_input, age = validate_age(name)
			game = FrogWorld if age < 18 else WizardWorld
			enviroment = GameEnviroment(game(name))
			enviroment.play()
			
  
  抽象工厂实现（abstrac_factory.py）的完整代码如下：
  
  
  该程序的简单输出如下：
  
  试着扩展游戏使它更加复杂。你的思想有多远就可以走多远：更多的障碍物，更多的敌人，以及任何你喜欢的东西。
  
#总结
  在这一章，我们已经见过如何使用工厂模式和抽象工厂设计模式。两种模式都可以在我们需要时使用（a）追踪对象创建，（b）分离对象的使用和创建，甚至（c）改善一个应用的性能和资源使用。情形（c）并不在章节中阐明。你可以把它当作一个不错的练习。
  
  工厂方法设计模式以一个不属于任何类的独立函数形式实现，它负责单一种类的对象（模型，接口，等等）的创建。我们看了工厂方法如何关联到玩具厂，注意到它如何被Django用来创建不同的表单字段，讨论了它的其他可能使用场合。像一个例子那样，我们实现了一个具有访问XML和JSON格式文件的工厂方法。
  
  抽象工厂设计模式通过一组属于一个单独类的工厂方法，它用于创建关联对象的家族（汽车的零部件，游戏的环境，等等）。我们注意到抽象工厂如何与汽车制造关联起来，Django包django_factory如何使用它去创建洁净测试，并覆盖了它的使用案例。抽象工厂的实现是一个向我们如何在一个单一类中使用多个相关工厂的小游戏。
  
  在下一章，我们会谈及生成器模式，它是另外一种可以用于复杂对象创建的精细控制的创建模式。
  
