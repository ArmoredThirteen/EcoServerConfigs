pipeline {
	agent any

	options {
		buildDiscarder(logRotator(numToKeepStr:'3'))
		disableConcurrentBuilds()
		timeout(time: 60, unit:'SECONDS')
	}

	stages {
		stage('Deploy'){steps {stageDeploy()}}
	}

	post {always {postAlwaysCleanup()}}
}


void stageDeploy() {
	if (!isMain())
		return

	sh "ls"

	sh "ssh -i ~/.ssh/GameSave_JenkinsBuild root@128.199.0.134 rm -rf /root/Eco/Storage"

	String[] filesToMove = [
		"./Network.eco",
		"./Users.eco",
		"./Voice.eco",
		"./WorldGenerator.eco",
	]

	for (int i = 0; i < filesToMove.length; i++) {
		scpToTarget(filesToMove[i], "/root/Eco/Configs");
	}

	sh "ssh -i ~/.ssh/GameSave_JenkinsBuild root@128.199.0.134 \"pkill EcoServer ; cd ~/Eco ; ./EcoServer\""
}

void postAlwaysCleanup() {
	echo "Cleanup"
	cleanWs()
}


boolean isMain(){
	return BRANCH_NAME.equals("main")
}


void scpToTarget(String filename, String dir) {
	sh "scp -i ~/.ssh/GameSave_JenkinsBuild ${filename} root@128.199.0.134:${dir}"
}
