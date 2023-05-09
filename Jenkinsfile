pipeline {
  agent any
  stages {
    stage('检出') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: GIT_REPO_URL,
            credentialsId: CREDENTIALS_ID
          ]]])
        }
      }

      stage('下载七牛插件') {
        steps {
          sh 'wget https://devtools.qiniu.com/qshell-v2.10.0-linux-386.tar.gz'
          sh 'tar -xzvf qshell-v2.10.0-linux-386.tar.gz'
          sh 'ls -alh'
        }
      }
      stage("上传 public") {
        steps {
            sh './qshell account ${QINIU_AK} ${QINIU_SK} my'
            sh './qshell qupload2 --bucket wechat-video-corly-cc --src-dir public --overwrite'
            sh 'echo "https://wechat-video.corly.cc/" > clearcdnurl.log'
            sh './qshell cdnrefresh -i clearcdnurl.log'
        }
      }

    }
  }