
简单的训练脚本,目前效果很 **不** 理想

```bash
#/bin/env bash


set -x

ID=c003
############### 必选参数 ############
# 正样本文件夹
posFolder=img
# 负样本文件夹
negFolder=bg
# 正样本数,自动算出
numPos=200
# 负样本数
numNeg=5000

bgFile=bg.txt
infoFile=${ID}.info
vecFile=${ID}.vec
cascadeFile=${ID}.cascade


############# 可选参数 #############
# 是否显示样本 -show 或者留空
isShow=''

############## 执行动作 #############
# 生成正样本描述文件
find ${posFolder} -name '*.png' -exec identify -format '%i 1 0 0 %w %h\r\n' \{\} \; \
	> ${infoFile}

if [ ${numPos} -eq 0 ] ; then
	numPos=$(cat ${infoFile} |wc -l)
fi
# 生成正样本特征文件 vec 文件
opencv_createsamples \
 	-info ${infoFile} \
	-vec ${vecFile} \
	-num ${numPos}	\
	${isShow}

# 生成负样本描述文件
find ${negFolder} -name '*.png' > ${bgFile}
# 如果没有指定就用数量
if [ ${numNeg} -eq 0 ] ; then
	numNeg=$(cat ${bgFile} |wc -l)
fi

# 训练
# opencv_traincascade

if [ 1 -eq 2 ] ; then

	opencv_haartraining \
		-data ${cascadeFile} \
		-vec ${vecFile} \
		-bg ${bgFile} \
		-npos ${numPos} \
		-nneg ${numNeg} \
		-nstages 10 \
		-mode ALL  
	# 检测
	../test  --face_cascade="resource/${cascadeFile}.xml" > /dev/null

else

	rm -rf  ${cascadeFile}
	mkdir -p ${cascadeFile}
#../opencv/release/bin/
	../opencv/release/bin/opencv_traincascade \
		-data ${cascadeFile} \
		-vec ${vecFile} \
		-bg ${bgFile} \
		-numPos ${numPos} \
		-numNeg ${numNeg} \
		-numStages 20 \
		-maxFalseAlarmRate 0.5 \
		-featureType LBP \
		-precalcValBufSize 4096 \
		-precalcIdxBufSize 4096 \
		-mode ALL  

	# 尝试不同算法 -featureType LBP \
	# 多线程 -numThreads 4 \
	# 可以减少最小命中率	-minHitRate 0.995 \
	# 或者增大最到失败率	-maxFalseAlarmRate 0.5 \
	echo "生成" ${cascadeFile} "OK"

	# 检测
	../test  \
		--face_cascade="resource/${cascadeFile}/cascade.xml" > /dev/null

fi

```