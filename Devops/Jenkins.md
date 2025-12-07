
基本结构
```groovy
pipeline { // 标准开头
	agent any // 表示jenkins cluster中任意的agent
	stages("build") { // 阶段
		steps {
			echo 'building the application...'
		}
	}
	stages("test") {
		steps {
			echo 'testing the application'
		}
	}
	stages("deploy") {
		steps {
			echo 'deploying the application'
		}	
	}
}
```
