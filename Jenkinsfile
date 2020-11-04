pipeline{
  agent any
  options{
    buildDiscarder(
      logRotator(daysToKeepStr: '5', numToKeepStr: '5')
    )
    disableConcurrentBuilds()
  }

parameters {
    choice(choices: 'dev\nqa2\nuat\nprod\n', description: 'Which environment?', name: 'DEPLOY_ENV_CHOICE')
    extendedChoice(
    name: 'tagName',
    defaultValue: '',
    description: 'tag name',
    type: 'PT_SINGLE_SELECT',
    groovyScript: """def gettags = ("git ls-remote -t https://github.com/prawalsaxena/jenkins1.git").execute()
        return gettags.text.readLines().collect { it.split()[1].replaceAll('refs/tags/', '').replaceAll("\\\\^\\\\{\\\\}", '')}
                    """,)
    }



  environment{
      def GENERATED_BUILD_TAG=''
  }

  stages{
    stage('parameterBuild'){
      steps{
        script{
          echo 'Setting job parameters'
        }
      }
    }
    stage('Tag Name '){
      stages{

        stage('test'){
          steps{
            script{
              echo "running on tag ${params.tagName}"
            }
          }
        }
      }//run stages
      post{
        success{
          script{
          configuration()
          }
        }
      }
    }//stage run
  }//job stages
}//pipeline
   
def configuration() {
  stage('Stage: Configuration')
  {
    sh """
      ls -ltrh
      """
  }
}
