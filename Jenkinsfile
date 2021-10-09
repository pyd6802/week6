podTemplate(containers: [
    containerTemplate(
        name: 'gradle',
        image: 'gradle:6.3-jdk14',
        command: 'sleep',
        args: '30d'
        ),
   ]) {
     node(POD_LABEL) {
         stage('Run pipeline against a gradle project') {
             git 'https://github.com/pyd6802/week6.git'
             container('gradle') {
                 stage('Build a gradle project') {
                     sh '''
                     cd Chapter08/sample1
                     chmod +x gradlew
                     ./gradlew test
                    '''
                    }

                 stage("Code coverage") {
                     try {
                    sh '''
                    pwd
                    cd Chapter08/sample1
                    ./gradlew jacocoTestCoverageVerification
                    ./gradlew jacocoTestReport
                    '''
                    } catch(e) {
                        echo "Code Coverage Failed"
                    }
                    publishHTML (target: [
                      reportDir: 'Chapter08/sample1/build/reports/jacoco/test/html',
                      reportFiles: 'index.html',
                      reportName: "JaCoCo Report"
                    ])
                 }
                 stage("Check style ") {
                    try { 
                    sh '''
                    pwd
                    cd Chapter08/sample1
                    ./gradlew checkstyleMain 
                    '''
                    } catch(e) {
                       echo "Checkstyle fails"
                    } 
                    publishHTML (target: [
                      reportDir: 'Chapter08/sample1/build/reports/checkstyle',
                      reportFiles: 'main.html',
                      reportName: "Checkstyle Report"
                    ])
                 }
           }
       }
   }
 }