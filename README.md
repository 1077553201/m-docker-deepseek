# m-docker-deepseek
docker部署本地deep seek


*****//创建容器的时候必须注意所有配置要符合自己的设定，创建完成之后，后面改很麻烦？？？*****
*****//要学会！要学会！要学会！仔细看官方文档!!!!!!!!!!!!!*****

//ollama 配置 并启动（本次缺少这个配置--network host ）

	docker run -d \
	--name ollama \
	-v ~/ollama-data:/root/.ollama \  # 挂载目录持久化模型数据
	-p 11434:11434 \                 # 暴露API端口
	--cpus=2 \                        # 限制使用2个CPU核心
	--memory=6g \                     # 分配6GB内存（预留2GB给Ubuntu系统）
	ollama/ollama:latest


参数说明​：

-v ~/ollama-data:/root/.ollama	#避免容器删除后模型丢失
-cpus=2 --memory=6g	#显式限制资源，防止Ollama耗尽虚拟机内存
----network host	#很重要？！？！？！？！？

//验证容器状态

	docker ps -a | grep ollama      # 状态应为"Up"
 	curl http://localhost:11434      # 返回"Ollama is running"即正常


//进入ollama 容器

	docker exec -it ollama /bin/bash

*//在ollama容器内运行

	//ollama 获取（pull）并运行deep seek R1 1.5b 蒸馏版 模型
	ollama run deepseek-r1:1.5b  #第三参数模型名称


//查看已经装好的模型

	llama list

//到这里基本装好deep seek-R1 1.5b 的版本可以在终端与之对话



//安装openWebUi

	docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main

//装好之后可以在虚拟机内部浏览器访问http://localhost:3000,配置连接deep seek使用******

******//目前遇到的问题：
因为创建时的疏忽忘加一个配置虚拟机外部访问不了，要解决此问题，需要重建容器完善配置（其实还有别的方法因为测试部署人员很懒没搞），持续改进中，因时间紧凑先就这样吧！！！
谁需要这个正确配置在外部访问的话让测试人员连夜给你们做！Q&Q？*****


![演示](https://github.com/1077553201/m-docker-deepseek/blob/main/yanshi%20.png?raw=true)





