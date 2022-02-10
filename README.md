# 模型主要思路
## &emsp; 模拟画家对于素描的绘制过程，将传统的人脸到素描之间一对一映射，变成了先描绘出来五官轮廓，再逐渐增加更多的细节，最后绘制出形象的人像素描。难点：利用前一步图生成的信息，辅助后一步生成器合成.
<br>
  <div align="center">
  <img src="https://github.com/George2413/face2sketch/blob/main/photo/1.png" width="500" height="300"/><br/>
 </div>
 
### &emsp; 为了让学习到的模型能够生成清晰的素描图, 我们选取了 25 张图片中最具有代表性的三个步骤图来构成我们的合成步骤的真实数据集.如上图所示。
<br>
  <div align="center">
  <img src="https://github.com/George2413/face2sketch/blob/main/photo/2.png" width="600" height="300"/><br/>
 </div>
 
 ### &emsp; 为了更好的利用素描信息，我们把人脸到素描的转换分成了两个阶段.如上图所示。
 ### &emsp; 每一个阶段内部实质上又分为三步，下面进行内部细节解释。
 
 <br>

  <div align="center">
  <img src="https://github.com/George2413/face2sketch/blob/main/photo/4.png" width="650" height="450"/><br/>
 </div>

 ### &emsp; 第一阶段和第二阶段内部的细节如上图所示。一共有6个生成器，6个判别器。在第一阶段中用到了人脸转素描图的G_AB和素描图转人脸的G_BA。在第二步合成中，到了第一阶段素描图片通过G1_BA合成的假人脸，和真实人脸一起送入第二阶段的生成器G2_AB,在第二步合成中，用到了第二阶段素描图片通过G2_BA合成的假人脸，和真实人脸一起送入第二阶段的生成器G3_AB。这一步的目的是更快得到人脸相关的素描信息，保存下来。

<br>
  <div align="center">
  <img src="https://github.com/George2413/face2sketch/blob/main/photo/3.png" width="650" height="500"/><br/>
 </div>
 
 ### &emsp; 在整个过程中, 每一步的判别器和生成器都是对抗关系，每次生成都利用了cycleGAN去合成图像，上图是第一阶段第一步的结构图，后面的每一步结构都与之类似。第一阶段的第二步和第三步区别只是在于把空白图换成素描图生成的真实人脸，第二阶段的区别在于和空白图换成了前一个阶段生成的素描人脸。
