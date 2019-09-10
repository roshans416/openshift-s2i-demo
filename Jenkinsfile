pipeline {
    agent {
      node {label 'maven'}
    }
environment {
    APP_NAME = "java-app-pipeline"
    GIT_REPO="http://github.com/roshans416/openshift-s2i-demo"
    GIT_BRANCH="master"
    STAGE_TAG = "promoteToQA"
    DEV_PROJECT = "roshan"
    PROD_PROJECT = "prod"
    //IMAGE_BUILDER = "maven-s2i-builder"
    IMAGE_BUILDER = "quay.io/roshantn/maven-s2i-builder"
    ARTIFACT_FOLDER = "target"
    PORT = 8080;
}
stages {
  stage('Get Latest Code') {
    steps {
      git branch: "${GIT_BRANCH}", url: "${GIT_REPO}" // declared in environment
    }
  }
//  stage('UNIT TESTS') {
//   steps {
//       sh "mvn test"
//    }
//}
  stage('PRE-BUILD') {
    when {
        expression {
            openshift.withCluster() {
            openshift.withProject(env.DEV_PROJECT) {
                return !openshift.selector("bc", "${APP_NAME}").exists();
                }
            }
        }
    }
    steps {
        script {
            openshift.withCluster() {
                openshift.withProject(env.DEV_PROJECT) {
                   openshift.newBuild("${GIT_REPO}", "--name=${APP_NAME}", "--image-stream=${IMAGE_BUILDER}", "--strategy=source")
                   //openshift.newBuild("${GIT_REPO}", "--name=${APP_NAME}")
                  //openshift.newBuild("${GIT_REPO}", "--name=${APP_NAME}", "--docker-image=${IMAGE_BUILDER}", "--strategy=source")
                }
            }
        }
    }
}

  stage('BUILD') {
  steps {
    script {
      openshift.withCluster() {
        openshift.withProject(env.DEV_PROJECT) {
          openshift.selector("bc", "${APP_NAME}").startBuild("--wait=true")
        }
      }
    }
  }  
}

  stage("DEPLOY TO DEV") {
  when {
      expression {
          openshift.withCluster() {
          openshift.withProject(env.DEV_PROJECT) {
              return !openshift.selector("dc", "${APP_NAME}").exists();
              }
          }
      }
  }
  steps {
    script {
        openshift.withCluster() {
            openshift.withProject(env.DEV_PROJECT) {
                def app = openshift.newApp("${APP_NAME}:latest")
                app.narrow("svc").expose("--port=${PORT}");
                def dc = openshift.selector("dc", "${APP_NAME}")
                while (dc.object().spec.replicas != dc.object().status.availableReplicas) {
                    sleep 10
                    }
                 }
            }
        }
    }
  }
}
}
