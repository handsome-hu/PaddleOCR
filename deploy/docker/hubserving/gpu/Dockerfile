# Version: 2.0.0
FROM registry.baidubce.com/paddlepaddle/paddle:2.0.0


# PaddleOCR base on Python3.7
RUN pip3.7 install --upgrade pip -i https://mirror.baidu.com/pypi/simple
RUN pip3.7 install paddlehub --upgrade -i https://mirror.baidu.com/pypi/simple

# 服务版与轻量版
#RUN git clone https://gitee.com/yarnk/bd-ocr-server.git /Paddle0CR
RUN git clone https://github.com/PaddlePaddle/PaddleOCR.git /PaddleOCR
WORKDIR /PaddleOCR

RUN pip3.7 install -r requirements.txt -i https://mirror.baidu.com/pypi/simple

RUN mkdir -p /PaddleOCR/inference/

# 下载解压模型开始:如果切换为服务端模型，记得修改deploy/hubserving/cr-ystem/params.py下对应的模型位置
# Download orc detect model(light version). if you want to change normal version, you can change ch_ppocr_mobile_v2.0_det_infer to ch_ppocr_server_v2.0_det_infer, also remember change det_model_dir in deploy/hubserving/ocr_system/params.py）
# 检测模型-推理模型：xxx_det_infer.tar
ADD https://paddleocr.bj.bcebos.com/PP-OCRv4/chinese/ch_PP-OCRv4_det_server_infer.tar /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ch_PP-OCRv4_det_server_infer.tar -C /PaddleOCR/inference/

# Download direction classifier(light version). If you want to change normal version, you can change ch_ppocr_mobile_v2.0_cls_infer to ch_ppocr_mobile_v2.0_cls_infer, also remember change cls_model_dir in deploy/hubserving/ocr_system/params.py）
# 文本方向分类模型-推理模型：cls_infer.tar
ADD https://paddleocr.bj.bcebos.com/dygraph_v2.0/ch/ch_ppocr_mobile_v2.0_cls_slim_infer.tar /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ch_ppocr_mobile_v2.0_cls_slim_infer.tar -C /PaddleOCR/inference/

# Download orc recognition model(light version). If you want to change normal version, you can change ch_ppocr_mobile_v2.0_rec_infer to ch_ppocr_server_v2.0_rec_infer, also remember change rec_model_dir in deploy/hubserving/ocr_system/params.py）
# 识别模型-推理模型：rec_infer.tar
ADD https://paddleocr.bj.bcebos.com/PP-OCRv4/chinese/ch_PP-OCRv4_rec_server_infer.tar /PaddleOCR/inference/
RUN tar xf /PaddleOCR/inference/ch_PP-OCRv4_rec_server_infer.tar -C /PaddleOCR/inference/

EXPOSE 8868

CMD ["/bin/bash","-c","hub install deploy/hubserving/ocr_system/ && hub serving start -m ocr_system"]
