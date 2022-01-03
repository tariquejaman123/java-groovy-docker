node('master')   {
      def dockerImageName= 'tjaman/javadedockerapp_$JOB_NAME:$BUILD_NUMBER'
      stage('SCM Checkout'){
         git 'https://github.com/zafar90/java-groovy-docker.git'          
      }
      stage('Build'){
         // Get maven home path and build
         //def mvnHome =  tool name: 'Maven-3.0.5-17', type: 'Apache'   
         sh "/opt/maven/bin/mvn package -Dmaven.test.skip=true"
         //sh " mvn package -Dmaven.test.skip=true"
      }       
     
     stage ('Test'){
         //def mvnHome =  tool name: 'Maven-3.0.5-17', type: 'Apache'    
         sh "/opt/maven/bin/mvn verify; sleep 3"
         //sh "mvn verify -Dmaven.test.skip=true"
      }
      
     stage('Build Docker Image'){         
           sh "docker build -t ${dockerImageName} ."
      }  
   
      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
              sh "docker login -u intdoc89 -p ${dockerPWD}"
         }
        sh "docker push ${dockerImageName}"
      }
      
    stage('Run Docker Image'){
            def dockerContainerName = 'javadockerapp_$JOB_NAME_$BUILD_NUMBER'
            def changingPermission='sudo chmod +x stopscript.sh'
            def scriptRunner='sudo ./stopscript.sh'           
            def dockerRun= "sudo docker run -p 8082:8080 -d --name ${dockerContainerName} ${dockerImageName}" 
            withCredentials([string(credentialsId: 'deploymentserverpwd', variable: 'jenkins')]) {
                  //sh "ssh -i ${jenkins} ssh -o StrictHostKeyChecking=no jenkins@13.127.81.47" 
                  //sh "ssh -i ${jenkins} scp -r stopscript.sh ec2-user@13.127.81.47:/home/ec2-user" 
                    //sh "sudo cp -r stopscript.sh /home/ubuntu" 
                  //sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no ec2-user@13.127.81.47 ${changingPermission}"
                    //sh "${changingPermission}"
                  //sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no ec2-user@13.127.81.47 ${scriptRunner}"
                    //sh "${scriptRunner}"
                  //sh "sshpass -p ${dpPWD} ssh -o StrictHostKeyChecking=no ec2-user@13.127.81.47 ${dockerRun}"
                    sh "${dockerRun}"
                  //sh "sudo kubectl apply -f app.yaml"
                    
            }
            
      
      }
      
         
  }
