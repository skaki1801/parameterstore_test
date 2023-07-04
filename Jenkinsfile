pipeline {
  agent any
  environment {
    AWS_REGION = 'eu-west-2'
  }
  parameters {
    string(name: 'PARAMETER_NAME', defaultValue: '', description: 'Enter the parameter store name')
    string(name: 'PARAMETER_VALUE', defaultValue: '', description: 'Enter the parameter store value')
    string(name: 'SECRET_NAME', defaultValue: '', description: 'Enter the secret name')
    string(name: 'SECRET_VALUE', defaultValue: '', description: 'Enter the secret value')
  }

  stages {
    stage('Create Parameters') {
      steps {
        script {
          def parameterName = params.PARAMETER_NAME
          def parameterValue = params.PARAMETER_VALUE

          withAWS(region: AWS_REGION, credentials: 'aws_creds') {
            def existingParameter = sh(returnStatus: true, script: "aws ssm get-parameter --name ${parameterName}")

            if (existingParameter == 0) {
              updateParameter(parameterName, parameterValue)
            } else {
              createParameter(parameterName, parameterValue)
            }
          }
        }
      }
    }

    stage('Create Secrets') {
      steps {
        script {
          def secretName = params.SECRET_NAME
          def secretValue = params.SECRET_VALUE

          withAWS(region: AWS_REGION, credentials: 'aws_creds') {
            def existingSecret = sh(returnStatus: true, script: "aws secretsmanager describe-secret --secret-id ${secretName}")

            if (existingSecret == 0) {
              updateSecret(secretName, secretValue)
            } else {
              createSecret(secretName, secretValue)
            }
          }
        }
      }
    }
  }
}

def updateParameter(parameterName, parameterValue) {
  sh "aws ssm put-parameter --name ${parameterName} --value ${parameterValue} --type String --overwrite"
}

def createParameter(parameterName, parameterValue) {
  sh "aws ssm put-parameter --name ${parameterName} --value ${parameterValue} --type String"
}

def updateSecret(secretName, secretValue) {
  sh "aws secretsmanager update-secret --secret-id ${secretName} --secret-string ${secretValue}"
}

def createSecret(secretName, secretValue) {
  sh "aws secretsmanager create-secret --name ${secretName} --secret-string ${secretValue}"
}
