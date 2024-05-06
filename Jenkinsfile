pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package' 
      }
    }

    stage('Test') {
      steps {
        // 运行测试并生成 Surefire 报告
        sh 'mvn -B test'
      }
      post {
        // 将 Surefire 报告存档
        always {
          archiveArtifacts artifacts: '**/target/surefire-reports/*.xml', fingerprint: true
        }
      }
    }

    stage('Generate Javadoc') {
      steps {
        // 生成 Javadoc
        sh 'mvn javadoc:jar'
      }
      post {
        // 将 Javadoc jar 存档
        always {
          archiveArtifacts artifacts: '**/target/*-javadoc.jar', fingerprint: true
        }
      }
    }

    stage('pmd') {
      steps {
        sh 'mvn pmd:pmd'
      }
    }
  }

  post {
    always {
      // 已有的存档命令保持不变
      archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
      archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
      archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
    }
  }
}
