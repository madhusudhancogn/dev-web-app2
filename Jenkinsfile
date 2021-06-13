node('master')
        {
            def mavenHome = tool name: "maven3.8.1"

            echo "GitHub BranhName ${env.BRANCH_NAME}"
            echo "Jenkins Job Number ${env.BUILD_NUMBER}"
            echo "Jenkins Node Name ${env.NODE_NAME}"

            echo "Jenkins Home ${env.JENKINS_HOME}"
            echo "Jenkins URL ${env.JENKINS_URL}"
            echo "JOB Name ${env.JOB_NAME}"

            stage('Poll SCM')
                    {
                        git branch: 'develop', credentialsId: '1b70f72e-2e9f-461a-9ae5-472619c69922',
                                url: 'https://github.com/madhusudhancogn/dev-web-app2.git'
                    }

            stage('Build')
                    {
                        sh "${MAVEN_HOME}/bin/mvn clean package"
                    }

            stage('Sonar Scan')
                    {
                        sh "${MAVEN_HOME}/bin/mvn sonar:sonar"
                    }

            stage('Upload Artifacts')
                    {
                        sh "${MAVEN_HOME}/bin/mvn deploy"
                    }

            stage('Deploy Artifacts Into Tomcat')
                    {
                        sshagent(['6e48e8ae-fa2a-4ddb-952e-da672794b5ce']) {
                            sh "scp -o  StrictHostKeyChecking=no target/dev-web-app2.war ec2-user@3.138.111.93:/opt/apache-tomcat-9.0.45/webapps"
                        }
                    }

            stage('Send Email Notification') {
                emailext body: '''Build Triggered
               Thanks,
               Madhu G''', subject: 'Jenkin Build Status', to: 'techmadhu63021@gmail.com'
            }
        }

