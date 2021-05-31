our model learns to deform a mesh from a mean shape to the target geometry 
我们的模型学习将网格从平均形状变形为目标几何形状

it provides the chance to encode any prior knowledge to the initial mesh  
它提供了将任何先验知识编码到初始网格的机会

### Challenge:

> 1. The first challenge is how to represent a mesh model, which is essentially an irregular graph, in a neural network and still be capable of extracting shape details effectively from a given color image represented in a 2D regular grid.   
>    第一个挑战是如何在神经网络中表示本质上是不规则图形的网格模型，并且仍然能够有效地从以2D规则网格表示的给定彩色图像中提取形状细节。
>
>    On the 3D geometry side, build a graph based fully convolutional network (GCN)on the mesh model, where the vertices and edges in the mesh are directly represented as nodes and connections in a graph. 
>    Network feature encoding information for 3D shape is saved on each vertex.    
>    在3D几何体方面，在网格模型上构建基于图的全卷积网络（GCN），其中网格中的顶点和边直接表示为图中的节点和连接。3D形状的网络要素编码信息保存在每个顶点上。
>
>    On the 2D image side, we use a VGG-16 like architecture to extract features as it has been demonstrated to be successful for many tasks  
>    在2D图像方面，我们已使用类似于VGG-16的体系结构来提取特征，因为事实证明它可以成功完成许多任务
>
>    To bridge these two, we design a perceptual feature pooling layer which allows each node in the GCN to pool image features from its 2D projection on the image, which can be readily obtained by assuming known camera intrinsic matrix.  
>    为了桥接这两者，我们设计了一个感知特征池化层，该层允许GCN中的每个节点从其在图像上的2D投影池化图像特征，这可以通过假设已知的相机内参矩阵轻松获得。
>
>    
>
> 2. Given the graph representation, the next challenge is how to update the vertex location effectively towards ground truth.  
>    给定图表示形式，下一个挑战是如何有效地朝着 ground truth 更新顶点位置。
>
>    we design a graph unpooling layer, which allows the network to initiate with a smaller number of vertices and increase during the forward propagation.  
>    我们设计了一个图形解池层，该层允许网络以较少数量的顶点开始，并在正向传播过程中增加。
>
>    With fewer vertices at the beginning stages, the network learns to distribute the vertices around to the most representative location, and then add local details as the number of vertices increases later. 
>    在开始阶段，顶点数较少时，网络将学习将顶点分布到最有代表性的位置，然后随着顶点数的增加而添加局部详细信息。  
>
>    use a deep GCN enhanced by shortcut connections as the backbone of our architecture, which enables large receptive fields for global context and more steps of movements  
>    使用通过快捷连接增强的深层GCN作为我们体系结构的主干，从而为全局环境和更多的移动步骤提供了广阔的接收范围

### Loss function

> a surface normal loss to favor smooth surface; 
> **表面法线损失**以有利于光滑表面；
>
> an edge loss to encourage uniform distribution of mesh vertices for high recall; 
> **边缘损失**用来促进网格顶点的均匀分布以提高召回率
>
> a laplacian loss to prevent mesh faces from intersecting each other.   
> **拉普拉斯损失**以防止网格面彼此相交