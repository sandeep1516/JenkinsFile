pipeline {
    agent {
        node {
        label 'Build_server'
        //customWorkspace '/home/sandeep/'
        }
    } 
    stages {
        stage ('checkout') {
            steps {
                echo "checking out from github.."
	     checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'bf7c9d3f-c2ef-4175-a3e0-1a57a6f84a64', url: 'https://github.com/sandeep1516/JenkinsFile.git']]])
       // checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/sandeep1516/consumer-borewell.git']]])
            }
        }
        stage ('build') {
            steps {
                echo "building the code"
                sh '/opt/maven/bin/mvn clean install'
                echo "build sucess"
            }
        }
        stage ('Artifactory upload') {
            steps {
                script {
                    def server = Artifactory.newServer url:'http://13.233.46.192:8081/artifactory', username:'admin',password:'admin-admin'
                    def uploadSpec = """{
                          "files":[
                          {
                               "pattern": "**/*.war",
                                "target": "example-repo-local",
                                "props": "p1=v1;p2=v2"
                          }
                        ]
                    }"""
                    def buildInfo1 = server.upload spec: uploadSpec
                    server.publishBuildInfo buildInfo1
                    echo "completed with uploading and now time for downloading"
                }
            }
        }
        stage ('download war from artifact') {
            steps {
                    echo "downloading from artifactory"
/**               
			   script {
                    //def server = Artifactory.server "http://18.144.57.124:8081/artifactory"
                    def server = Artifactory.newServer url:'http://13.127.133.216:8081/artifactory/example-repo-local', username:'admin',password:'admin-admin'
                    def downloadSpec = """{
						"files": [
						{
							"pattern": "*.*",
							"target": "/home/." 
							}
						]
					}"""
					//def buildInfo1 = server.download downloadSpec
					def buildInfo1 = server.download spec: downloadSpec
					echo 'steppppp'
                    //server.publishBuildInfo buildInfo1
                }
**/
            }        
        }
        
    }
}
