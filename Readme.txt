Without a Dockerfile
If you don't want to include a Dockerfile in your project, it is sufficient to do the following:

$ docker run -dit --name my-apache-app -p 80:80 -v "$PWD":/usr/local/apache2/htdocs/ httpd:2.4

---Directly run without building---



pipeline{ 
    agent any
    stages{ 
        stage ('parallel-stages'){ 
            parallel{
        stage ('master-branch'){
            agent{ 
            node { 
                label "built-in"
                customWorkspace "/mnt/project1"
                 }
                 }
            steps {
                sh "rm -rf /mnt/project1/*"
                sh "git clone https://github.com/utkarsh-darhe/GIT-DOCKER-REPO.git -b master"
		sh "chmod -R 777 /mnt/project1/GIT-DOCKER-REPO"
	//	sh "docker build -t httpd-master /mnt/project1/GIT-DOCKER-REPO "
		sh "docker run -dit --name master -p 80:80 -v /mnt/project1/GIT-DOCKER-REPO:/usr/local/apache2/htdocs/ httpd:2.4"
		          }
	                           } 
	          
	        stage ('Dev-branch'){
            agent{ 
            node { 
                label "built-in"
                customWorkspace "/mnt/project2"
                 }
                 }
            steps {
                sh "rm -rf /mnt/project2/*"
                sh "git clone https://github.com/utkarsh-darhe/GIT-DOCKER-REPO.git -b Dev"
		sh "chmod -R 777 /mnt/project2/GIT-DOCKER-REPO"
	//	sh "docker build -t httpd-Dev /mnt/project2/GIT-DOCKER-REPO "
		sh "docker run -dit --name Dev -p 100:80 -v /mnt/project2/GIT-DOCKER-REPO:/usr/local/apache2/htdocs/ httpd:2.4"
		          }
	                           }   
	                           
	        stage ('QA-branch'){
            agent{ 
            node { 
                label "built-in"
                customWorkspace "/mnt/project3"
                 }
                 }
            steps {
                sh "rm -rf /mnt/project3/*"
                sh "git clone https://github.com/utkarsh-darhe/GIT-DOCKER-REPO.git -b QA"
		sh "chmod -R 777 /mnt/project3/GIT-DOCKER-REPO"
	//	sh "docker build -t httpd-QA /mnt/project3/GIT-DOCKER-REPO "
		sh "docker run -dit --name QA -p 90:80 -v /mnt/project3/GIT-DOCKER-REPO:/usr/local/apache2/htdocs/ httpd:2.4"
		          }
	                           }                      
	                           
	                           
	                           
	                           
	                           
	                           
	                } 
	                                 } 
	                                
	                                
	                                 
	                                 
} 
}
