Step 1

java -jar jenkins.war --httpPort=7080
cd C:\Users\Admin\Desktop\MProject

git init

MProject

git add .

git commit -m "Initial commit for MProject"

git remote add origin https://github.com/nhatthai2004/MProject.git

git push -u origin master https://github.com/nhatthai2004/MProject.git

Step 2
Chrome or any other web browser
http://localhost:8080

If install in C:, then
C:\Program Files (x86)\Jenkins\secrets\initialAdminPassword
Me
C:\ProgramData\Jenkins\.jenkins\secrets
717c1d79d8294604bb89c523ba5bfcef

Copy to the place, then simply do the recommended install option

Step 3
Create a job

MobileSAM-Pipeline

Pipeline script from SCM

Git

Username and Pass

Branch Specifier
*/master

cd C:\Users\Admin\Desktop\MProject
echo pipeline { > Jenkinsfile

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
            post {
                always {
                    echo 'Archiving test results...'
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to the test environment...'
                sh 'docker-compose up -d'
            }
        }

        stage('Release') {
            steps {
                echo 'Releasing to production...'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished. Sending notifications...'
        }
    }
}

git add Jenkinsfile
git commit -m "Added Jenkinsfile for Jenkins pipeline"
git push origin master

Step 4
install plugin
SonarQube Scanner

Step 5
c95ca4d96f39507fe7adf29b0a2ea9822bb0de6dc65c25a7de14e99c7cb904d2cfd2c2887d8a62e0011a5733829bcfafe86cc65669813e01d9d2cd154dd25b8b5ab07adc7e54862b2982f99b24a1002ab263dbb1c2033b8ee97351da08af326f6b8410720e07a2ceab95295b970fce567c4165dabc2c81d42e0675fc8dd629fb

Maven

# Step 1: Set Java version
$javaVersion = "11"
$javaUrl = "https://download.oracle.com/java/$javaVersion/latest/jdk-$javaVersion_windows-x64_bin.zip"
$javaZipPath = "C:\tools\jdk.zip"
$javaInstallPath = "C:\Program Files\Java"

# Download and extract JDK (if not installed)
if (!(Test-Path -Path $javaInstallPath)) {
    Write-Host "Downloading and Installing Java JDK..."
    Invoke-WebRequest -Uri $javaUrl -OutFile $javaZipPath
    Expand-Archive -Path $javaZipPath -DestinationPath $javaInstallPath
    Remove-Item $javaZipPath
}

# Step 2: Download Maven
$mavenVersion = "3.8.7"
$mavenUrl = "https://dlcdn.apache.org/maven/maven-3/$mavenVersion/binaries/apache-maven-$mavenVersion-bin.zip"
$mavenZipPath = "C:\tools\maven.zip"
$mavenInstallPath = "C:\Program Files\Maven"

# Download Maven (if not installed)
if (!(Test-Path -Path $mavenInstallPath)) {
    Write-Host "Downloading and Installing Maven..."
    Invoke-WebRequest -Uri $mavenUrl -OutFile $mavenZipPath
    Expand-Archive -Path $mavenZipPath -DestinationPath $mavenInstallPath
    Remove-Item $mavenZipPath
}

# Step 3: Set Environment Variables
[System.Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\Java\jdk-$javaVersion", "Machine")
[System.Environment]::SetEnvironmentVariable("M2_HOME", "C:\Program Files\Maven\apache-maven-$mavenVersion", "Machine")
[System.Environment]::SetEnvironmentVariable("Path", "$env:Path;C:\Program Files\Java\jdk-$javaVersion\bin;C:\Program Files\Maven\apache-maven-$mavenVersion\bin", "Machine")

# Step 4: Verify Installation
$mavenVersionOutput = & mvn -version
if ($mavenVersionOutput) {
    Write-Host "Maven installed successfully"
    Write-Host $mavenVersionOutput
} else {
    Write-Host "Maven installation failed"
}

# Refresh environment variables in the current session
$env:JAVA_HOME = "C:\Program Files\Java\jdk-$javaVersion"
$env:M2_HOME = "C:\Program Files\Maven\apache-maven-$mavenVersion"
$env:Path += ";C:\Program Files\Java\jdk-$javaVersion\bin;C:\Program Files\Maven\apache-maven-$mavenVersion\bin"

# Confirm Java installation
Write-Host "Java Version:"
& java -version







