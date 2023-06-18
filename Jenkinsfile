def PROJECT_NAME = "${JOB_BASE_NAME}".replace("/", "_")
def PROJECT_PATH = "/var/jenkins_home/workspace/Test"
pipeline {
    agent { label 'master' }
    environment {
        // -- TOFAS
        TOFAS_DB                = "tofasdb"
        TOFAS_DEPLOYMENT_NAME   = "cxplus-tofas"

        // -- SKODA
        SKODA_DB                = "skodadb"
        SKODA_DEPLOYMENT_NAME   = "cxplus-skoda"

        // -- ARCELIK
        ARCELIK_DB              = "arcelikdb"
        ARCELIK_DEPLOYMENT_NAME = "cxplus-arcelik"

        // -- TEST
        TEST_DB              = "testdb"
        TEST_DEPLOYMENT_NAME = "cxplus-test"
    }

    // ----------------

    stages {
        stage('Initialize variables') {
            steps {
                echo 'Building Container..'

                script {
                    if (PROJECT_NAME == 'test') {
                        DATABASE_NAME=TEST_DB
                        DEPLOYMENT_NAME=TEST_DEPLOYMENT_NAME
                    if (PROJECT_NAME == 'tofas-deneme') {
                        DATABASE_NAME=TOFAS_DB
                        DEPLOYMENT_NAME=TOFAS_DEPLOYMENT_NAME
                    } else if (PROJECT_NAME == 'skoda-deneme') {
                        DATABASE_NAME=SKODA_DB
                        DEPLOYMENT_NAME=SKODA_DEPLOYMENT_NAME
                    } else if (PROJECT_NAME == 'arcelik-deneme') {
                        DATABASE_NAME=ARCELIK_DB
                        DEPLOYMENT_NAME=ARCELIK_DEPLOYMENT_NAME
                    }
                }
                echo 'Project Name:' + PROJECT_NAME
                // -- DATABASE NAME ENV.
                sh "export DATABASE_NAME=" + DATABASE_NAME + " && envsubst < " + PROJECT_PATH + "/" + PROJECT_NAME + "/appsetting.json > .appsetting.json"
                sh "mv " + PROJECT_PATH + "/" + PROJECT_NAME + "/.appsetting.json " + PROJECT_PATH + "/" + PROJECT_NAME + "/appsetting.json"
                // -- K8S DEPLOYMENT ENV.
                sh "export DEPLOYMENT_NAME=" + DEPLOYMENT_NAME + " && envsubst < " + PROJECT_PATH + "/" + PROJECT_NAME + "/deployment.yaml > .deployment.yaml"
                sh "mv " + PROJECT_PATH + "/" + PROJECT_NAME + "/.deployment.yaml " + PROJECT_PATH + "/" + PROJECT_NAME + "/deployment.yaml"
            }
        }
    }
}
