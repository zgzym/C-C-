Opencv中的SVD分解函数为cv::SVD:compute(InputArray src, OutputArray w, OutputArray U, OutputArray VT, int flags = 0)

其中，src为待分解的矩阵；

w为奇异值向量，列数为1；

U为左侧特征向量矩阵

VT为右侧特征向量矩阵的复共轭转置

与matlab中计算的U、V矩阵存在符号差异，并且特征值的排列顺序可能会不同
