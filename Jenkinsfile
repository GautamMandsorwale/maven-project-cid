{\rtf1\ansi\ansicpg1252\cocoartf1561\cocoasubrtf200
{\fonttbl\f0\fmodern\fcharset0 Courier;}
{\colortbl;\red255\green255\blue255;\red0\green0\blue0;}
{\*\expandedcolortbl;;\cssrgb\c0\c0\c0;}
\margl1440\margr1440\vieww10800\viewh8400\viewkind0
\deftab720
\pard\pardeftab720\partightenfactor0

\f0\fs26 \cf0 \expnd0\expndtw0\kerning0
pipeline \{\
    agent any\
    \
    parameters \{ \
         string(name: 'tomcat_dev', defaultValue: '35.166.210.154', description: 'Staging Server')\
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')\
    \} \
\
    triggers \{\
         pollSCM('* * * * *') // Polling Source Control\
     \}\
\
stages\{\
        stage('Build')\{\
            steps \{\
                sh 'mvn clean package'\
            \}\
            post \{\
                success \{\
                    echo 'Now Archiving...'\
                    archiveArtifacts artifacts: '**/target/*.war'\
                \}\
            \}\
        \}\
\
        stage ('Deployments')\{\
            parallel\{\
                stage ('Deploy to Staging')\{\
                    steps \{\
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@$\{params.tomcat_dev\}:/var/lib/tomcat7/webapps"\
                    \}\
                \}\
\
                stage ("Deploy to Production")\{\
                    steps \{\
                        sh "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@$\{params.tomcat_prod\}:/var/lib/tomcat7/webapps"\
                    \}\
                \}\
            \}\
        \}\
    \}\
\}}