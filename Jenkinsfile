def gitCommit() {
        sh "git rev-parse HEAD > GIT_COMMIT"
        def gitCommit = readFile('GIT_COMMIT').trim()
        sh "rm -f GIT_COMMIT"
        return gitCommit
    }

def answerQuestion = ''
def tokenNeeded = ''
def idApp = ''

    node {
        // Checkout source code from Git
        stage 'Checkout'
        checkout scm

/* //        clean all
        try {
          stage('Destroy') {
                sh "oc4 login --insecure-skip-tls-verify -u kubeadmin -p epCzU-meW75-inPMR-ELwix --server=https://api.upi.testkube.org:6443"
		sh "oc4 project pipeline-demo"

	  }
    }
          catch(e) {
                     build_ok = false
                     echo e.toString()
} 
*/

	// Analyse the code for vulnerabilities using SCA
  /* stage 'SCA'
	sh "/opt/Fortify/Fortify_SCA_and_Apps_19.1.0/bin/sourceanalyzer -b addressbook -clean"
	sh "/opt/Fortify/Fortify_SCA_and_Apps_19.1.0/bin/sourceanalyzer -b addressbook *.java"
	sh "/opt/Fortify/Fortify_SCA_and_Apps_19.1.0/bin/sourceanalyzer -b addressbook -scan -f adressbook.fpr"
	sh "/opt/Fortify/Fortify_SCA_and_Apps_19.1.0/bin/ReportGenerator -format xml -source addressbook.fpr -f test.xml"
	sh "/opt/Fortify/Fortify_SCA_and_Apps_19.1.0/bin/ReportGenerator -format rtf -source addressbook.fpr -f test.rtf"
	sh "cp test.rtf /var/lib/jenkins"
        sh "/opt/Fortify/Fortify_SCA_and_Apps_19.1.0/bin/fortifyclient -url http://192.168.12.191:8080/ssc -authtoken f11727d6-fd70-4dd1-b7e4-78bb03fc869d uploadFPR -file openshift-mf.fpr -project addressbook -version 1"
//        sh " http -f POST http://192.168.12.191:8080/ssc/upload/resultFileUpload.html?mat=MzRmM2QxZWUtMzVkMy00NDk0LThjZmMtNGMwMWYxMGM3OGQ4 entityId=26112 files[]@php-safe.fpr
    
        script {
          tokenNeeded = sh (returnStdout: true, script: '/opt/Fortify/Fortify_SCA_and_Apps_19.1.0/bin/fortifyclient -url http://192.168.12.191:8080/ssc token -gettoken AnalysisUploadToken -user admin -password redhat12redhat |cut -c 22-57')
        }
        script {
          idApp = sh (returnStdout: true, script: '/opt/Fortify/Fortify_SCA_and_Apps_19.1.0/bin/fortifyclient -url http://192.168.12.191:8080/ssc -authtoken "$tokenNeeded" listApplicationVersions |grep php-safe')
        }
        script {
          answerQuestion = sh (returnStdout: true, script: 'xmllint --xpath "string(//GroupingSection/@count)" test.xml')
        } 
 
        echo "${tokenNeeded}"
	sh "curl -v -u admin:admin123 --upload-file test.rtf http://nexus.example.local/content/sites/safe/report`date +%F-%T`.rtf"
	echo "${answerQuestion}"
        if ( answerQuestion != "" ) {
	  echo 'SCM has some critical findings - terminating build... look at the rtf report @ http://nexus.example.local/content/sites/safe/'
	currentBuild.result = 'FAILURE'
	sh "exit ${answerQuestion}" 
	}
*/
      
        // Build the app 
        stage 'Build'
	sh "oc login --insecure-skip-tls-verify https://console.ocexternal.linuxthoughts.com:8443 --token=iGJuEzs1uEpKAaWi2bqdBvVIKKZfH31GMxBibpz1t2I"
        //sh "oc new-build https://github.com/roshans416/openshift-s2i-demo --name=java-app --strategy=source --docker-image=quay.io/roshantn/maven-s2
//i-builder --to-docker=true --to=quay.io/roshantn/java-app --push-secret=quay-secret"
         sh "oc project pipeline-demo"
	 sh "oc start-build java-app --wait=true"
	    
	 // Image scanning
	 
	 //stage 'IMAGE SCANNING'
	 //aquaMicroscanner imageName: 'busybox', notCompliesCmd: '', onDisallowed: 'ignore', outputFormat: 'html'

        // run the container
        stage 'Deploy'
        
        sh "oc project pipeline-demo"
    	//sh "oc4 adm  policy add-scc-to-user anyuid -z default"
        sh "oc new-app --docker-image=quay.io/roshantn/java-app:latest"
	sh "oc expose svc/java-app"
    }
