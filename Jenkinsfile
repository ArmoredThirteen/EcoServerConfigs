pipeline {
	agent any

	options {
		buildDiscarder(logRotator(numToKeepStr:'3'))
		disableConcurrentBuilds()
	}

	stages {
		stage('Deploy'){steps {stageDeploy()}}
	}

	post {always {postAlwaysCleanup()}}
}


void stageDeploy() {
	sh "ls"
	return

	if (!isMain())
		return

	String[] filesToMove = [
		"./Network.eco",
		"./Users.eco",
		"./Voice.eco",
		"./WorldGenerator.eco",
	]

	for (int i = 0; i < filesToMove.length; i++) {
		scpToTarget(filesToMove[i], "/root/Eco/Configs");
	}
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
