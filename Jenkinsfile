node {
    stage('Checkout'){
        echo 'Bajando los fuentes del repositorio...'
        checkout scm
    }
    stage ('Compile'){
        echo 'Compilando ...'
        executeMavenGoal('clean compile','pom.xml', '-Xmx256m')
        echo currentBuild.currentResult
    }
    stage ('Install') {
        echo 'Building ...'
        executeMavenGoal('install -Dmaven.test.skip=true', 'pom.xml', '-Xmx256m')
    }
    stage ('Deploy Adapter') {
        echo 'Deploying on MFP Server ...'
        executeAdapterDeploy('clean','pom.xml','-Xmx256m')
    }
}

/* Ejecuta comandos de maven */
def executeMavenGoal(pGoalsAndOptions, pPomFilePath, pMavenOpts){
    def mavenToolDefault = 'maven3.6.3'
    def javaToolDefault = 'JDK8'
    def mavenSettingsDefault = ''
    def mavenRepositoryDefault = '../../.m2'
    executeMavenGoal (mavenToolDefault, javaToolDefault, mavenSettingsDefault, mavenRepositoryDefault, pGoalsAndOptions, pPomFilePath, pMavenOpts)
}

/* Ejecuta comandos de maven */
def executeMavenGoal (pMavenToolName, pJdkToolName, pMavenSettingsId, pMavenRepositoryPath, pGoalsAndOptions, pPomFilePath, pMavenOpts) {
    withMaven(
         maven: pMavenToolName,
         //mavenSettingsConfig: pMavenSettingsId,
         mavenLocalRepo: pMavenRepositoryPath,
         jdk: pJdkToolName,
         mavenOpts: pMavenOpts
         ) {

         def mavenCommand = "mvn "

         if(null != pPomFilePath){
             mavenCommand = mavenCommand + " -f " + pPomFilePath
         }

         try {
            sh mavenCommand + " " + pGoalsAndOptions

         } catch (Exception err) {
            echo 'Maven clean install failed'
            currentBuild.result = 'FAILURE'
            throw err
         }

    }
}


/* Ejecuta comandos de maven */
def executeAdapterDeploy(pGoalsAndOptions, pPomFilePath, pMavenOpts){
    def mavenToolDefault = 'maven3.6.3'
    def javaToolDefault = 'JDK8'
    def mavenSettingsDefault = ''
    def mavenRepositoryDefault = '../../.m2'
    executeAdapterDeploy (mavenToolDefault, javaToolDefault, mavenSettingsDefault, mavenRepositoryDefault, pGoalsAndOptions, pPomFilePath, pMavenOpts)
}
/* Ejecuta comandos de maven */
def executeAdapterDeploy (pMavenToolName, pJdkToolName, pMavenSettingsId, pMavenRepositoryPath, pGoalsAndOptions, pPomFilePath, pMavenOpts) {
    withMaven(
         maven: pMavenToolName,
         //mavenSettingsConfig: pMavenSettingsId,
         mavenLocalRepo: pMavenRepositoryPath,
         jdk: pJdkToolName,
         mavenOpts: pMavenOpts
         ) {

         def mavenCommand = "mvn "

         if(null != pPomFilePath){
             mavenCommand = mavenCommand + " -f " + pPomFilePath
         }

         try {
             sh "mfpdev server add mfpServer --url http://10.0.1.31:9080 --login admin --password admin --setdefault"
             sh  "mfpdev adapter deploy mfpServer" 

         } catch (Exception err) {
            echo 'Deploy Adapter failed'
            currentBuild.result = 'FAILURE'
            throw err
         }

    }
}