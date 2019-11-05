

# 生成tfrecords文件
我们借助[](ai/uai-train/cases/crnn-chinese/imgprep)中的镜像来生成tfrecords文件。
## 本地生成tfrecords文件
我们可以将数据按照如下文件结构进行放置。

  * char\_dict.json、index\_2\_ord\_map.json和ord\_2\_index\_map.json需要放置在data/data/char\_dict文件夹下(这三个文件描述了字符的编码和训练中index的关系)；

  * Test和Train中放置了我们准备好的用于训练和测试的sample.txt文件；

  * ./pic中放置了生成的图像文件；

<code>
    /_ data/

  |_ data/
  |_ char_dict/
    |_ char_dict.json
    |_ index_2_ord_map.json
    |_ ord_2_index_map.json
  |_ Test/
    |_ sample.txt
  |_ Train/
    |_ sample.txt
  |_ pic/
   |_ tfrecords
  </code>

我们需要将本地的/data/data和/data/tfrecords映射到docker镜像中去

<code>
  sudo docker run -it -v /data/data:/data/data -v /data/data/tfrecords:/data/output uhub.service.ucloud.cn/uai_demo/crnn_cpu:latest  /bin/bash -c " /usr/bin/python /data/code/tools/write_text_tfrecords.py  --batch_size=10000"
  </code>
  然后我们可以在本地/data/tfrecords中找到生成的tfrecords文件。






