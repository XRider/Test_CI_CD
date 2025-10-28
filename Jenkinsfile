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
                    xcodebuild clean -workspace Test_CI_CD.xcodeproj -scheme YourApp -configuration Release
                    xcodebuild archive \
                      -workspace Test_CI_CD.xcodeproj \
                      -scheme YourApp \
                      -configuration Release \
                      -archivePath build/YourApp.xcarchive \
                      -destination 'generic/platform=iOS'
                    xcodebuild -exportArchive \
                      -archivePath build/YourApp.xcarchive \
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
