pipeline {
    agent any
    environment {
        LANG = "en_US.UTF-8"
        LC_ALL = "en_US.UTF-8"
        PATH = "/usr/local/bin:/opt/homebrew/bin:${PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "🔁 拉取代码"
                git branch: 'main', url: 'https://github.com/XRider/Test_CI_CD.git'
            }
        }

        stage('Build IPA') {
            steps {
                echo "🏗️ 使用 xcodebuild 构建 IPA"
                sh '''
                    xcodebuild clean -project Test_CI_CD.xcodeproj -scheme Test_CI_CD -configuration Release
                    xcodebuild archive \
                      -project Test_CI_CD.xcodeproj \
                      -scheme Test_CI_CD \
                      -configuration Release \
                      -archivePath build/Test_CI_CD.xcarchive \
                      -destination 'platform=MacOS,name=xzc的MacBook Pro'
                    xcodebuild -exportArchive \
                      -archivePath build/Test_CI_CD.xcarchive \
                      -exportOptionsPlist exportOptions.plist \
                      -exportPath build/
                '''
            }
        }

        stage('Archive IPA') {
            steps {
                archiveArtifacts artifacts: 'build/*.ipa', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "✅ 构建成功！IPA 已生成。"
        }
        failure {
            echo "❌ 构建失败，请检查日志。"
        }
    }
}
