场景：识别关键错误和可疑行为中的可解释性
主要关注点：公式，标准指标的推理和制定，对模型去噪原理的数学解释，四种基于属性的损失
基于扩散的修复？？
	
![[Pasted image 20241125154123.png]] 
扩散模型？？
	[[NeurIPS-2020-denoising-diffusion-probabilistic-models-Paper.pdf]]
	扩散概率模型：
		使用变分推理训练的参数化马尔可夫链，在有限时间内产生于数据匹配的样本
		先给图片加噪，然后根据模型实现去噪，神经网络学习去噪的过程
	[[扩散模型]]
要点：
	用正确属性衡量修复质量
	将广泛使用的属性正式化——》衡量异常检测器的解释质量
		异常检测器满足线性分解性：
			总体异常分数是特征异常分数的聚合
	使用生成模型来生成修复，然后根据检测器评估该修复
线性分解性？？
	线性空间中的一个向量分解成某些基向量的加和
	![[Pasted image 20241126144050.png]]
反事实解释？？
	[[2402.14469v1.pdf]]
	[[反事实解释]]
	其核心在于通过设想与事实相反的情况，来解释为何模型会做出特定的预测或决策
	![[Pasted image 20241126141921.png]]
与域无关的属性？？
	不依赖于特定领域的属性
属性引导的反事实解释生成：
![[Pasted image 20241126143854.png]]
四种不同的异常检测的异常特征分数的设置：
	![[Pasted image 20241126145219.png]]

语言建模的度量公式和视觉案例的度量公式是不同的
	针对LLM和针对CV的度量是不一样的
特征异常分数和特征归因分数相关
反事实解释的常见需求：
	整体改进：
		修复的版本应该根据定义修复异常
			![[Pasted image 20241125164446.png]]
	相似性
		修复应该类似于原始的数据
	局部改进
		在异常区域的地方发生改进，而不是在仅整体的异常分数降低
	非退化
		原本的非异常区域不应明显恶化
	![[Pasted image 20241126150045.png]]

使用尚需四个属性定义为目标函数，并使用风险约束化来构建问题
	将以上四种loss的风险约束的最优化
	![[Pasted image 20241126150302.png]]
	
![[Pasted image 20241125165208.png]]
 **对于视觉，目前领先的范式是扩散模型和生成对抗网络 。扩散模型也适用于时间序列数据 ，我们参考对其他技术的调查。** 
 形式属性引导扩散：
	 基于对前面扩散模型的理解：在使用基于属性的损失的每一步，鼓励最终的迭代，我们采取，更服从于我们的属性。其次，我们使用了**掩码填充**，以确保非异常区域通常由迭代保留。我们实现这个修改的迭代如下：从初始噪声开始
	 ![[Pasted image 20241126151152.png]]
	 ![[Pasted image 20241126151344.png]]
掩码填充？？
	![[Pasted image 20241126181618.png]][[2108.01073v2.pdf]]
	用掩码将非异常区域过滤掉，只改变修复异常区域
实验：
	消融实验，不同的超参如何影响修复质量，关注四种基于属性的损失的权重
	loss1，2，3，4
	![[Pasted image 20241126151821.png]]
	图片识别数据集：
	![[Pasted image 20241126151928.png]]
	时间序列数据集：
	在生成异常修复方面的性能。我们报告平均值和标准差值。由于各类之间的值范围可能很大，因此我们报告了引导式生成相对于基线的中位数改进  (∆)。
	![[Pasted image 20241126152348.png]]
	![[Pasted image 20241126152944.png]]
	
消融实验：
	图片分类中四个超参数尺度变化不会对指标产生显著影响。
	时间序列：
	笔记上文中四个超参数权重会不会影响四种属性loss
	![[Pasted image 20241126152614.png]]
 工作：
	 利用常见的异常检测器是线性可分解的事实，可以定义正式的、通用的、独立于域的属性以实现可解释性。使用这些属性，我们展示了如何使用属性引导的扩散设置生成高质量的反事实

按格式梳理：
	总结
		四个新的loss设置，反事实解释的四种需求
		四种不同的异常分数的设置
	研究主要内容：
		领域
			图片和时间序列的异常检测
			数据中可解释性
		场景
			基于修复的异常检测，进行反事实解释 统一背景进行反事实解释和基于修复的异常检测
		问题
			在具有形式属性的异常检测中生成和评估反事实解释很困难
		思路
			基于生成修复的方法在不同模式下用不同的线性分解的各个特征异常分数异常构建整体异常特征分数
			根据反事实解释的四种需求利用神经网络生成反事实解释
	技术
		见上文
	实验
		见上文
	特点
		异常修复和反事实解释
			异常修复有四种生成总体异常分数的场景
			反事实解释有四种需求构建出的四种loss