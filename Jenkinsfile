# i am adding the commend

pipeline{

   agent any 

   stages{
       stage("Checkout"){
        steps{
             git url: 'https://github.com/cloud-junction/devops-webapp-maven.git', branch: 'master'
             sh "ls -ll"
        }
       }
       stage("UnitTest"){
        steps{
            sh "mvn test"
        }
       }
       stage("Sonar Scan"){
        steps{
            /*
            sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=devops-pipe"
            */
            echo "SonarCheck"
        }
       }
       stage("Build"){

        steps{

            sh 'mvn package'
        }
       }
       stage("ArchiveArtifacts"){

        steps{
               archiveArtifacts artifacts: 'target/my-app.war', followSymlinks: false
        }
       }
       stage("NexusPublisher"){

        steps{

            echo "Uploading artifacts to Nexus"
        }
       }

       stage("DeployDev"){

        steps{

            echo "Deploying to Dev"
        }
       }

       stage("Approval"){

        steps{

            timeout(time: 15, unit: 'MINUTES'){ 
	               input message: 'Do you approve deployment for production?' , ok: 'Yes'}

        }
       }

       stage("ProdDeploy"){
        steps{

            sshPublisher(publishers: [sshPublisherDesc(configName: 'devserver', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'target', sourceFiles: 'target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
        }
       }

   } 
   post{

    failure{

             mail to:"cloud.junction.in@gmail.com",
             subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
             body: "${env.BUILD_URL} has result FAILURE " 

        
    }
   }


}
