pipeline{
  agent {node {label 'linux'}}
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


  stages{
    steps {
        checkout(
        changelog: false,
        poll: false,
        scm: [
          $class: 'GitSCM',
          branches: [[name: "${params.tagName}"]],
          doGenerateSubmoduleConfigurations: false,
          extensions: [
            [
              $class: 'RelativeTargetDirectory',
              relativeTargetDir: '.Build-Dir'
            ]
          ])
          }
    stage('Tag Name '){
      when{ expression{!params.SkipRun}}
      stages{

        stage('test'){
          steps{
            script{
              echo "running on tag ${TagName}"


            }
          }
        }
      }//run stages
      post{
        success{
          script{
            env.GENERATED_BUILD_TAG=build_tag
          }
        }
      }
    }//stage run
  }//job stages
}//pipeline

def getUserNameFromCause(currentBuild){
  def userCause
  try{
    userCause=currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')
    if (userCause==null || userCause.size()<1){
      return ''
    }
    userCause=userCause[0]
  }catch(all){
    return ''
  }
  return userCause.userName!=null?"${userCause.userName}":''
}
