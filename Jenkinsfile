pipeline {
   agent any

   stages {
      stage('Build') {
         steps {
            checkout scm
            sh "./gradlew clean build jacocoRootReport printcp"
         }

         post {
            success {
               metrics filePattern: '**/src/**/*.java', classPathFile: 'cp.txt'
               recordIssues(tools: [checkStyle(pattern: '**/checkstyle/*.xml'), java(), pmdParser(pattern: '**/pmd/*.xml')])
               publishCoverage adapters: [jacocoAdapter('build/reports/jacoco/jacocoRootReport/jacocoRootReport.xml')], sourceFileResolver: sourceFiles('NEVER_STORE')
            }
         }
      }
   }
}