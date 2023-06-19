pipeline {
  agent any
  environment {
      AWS_REGION = 'eu-west-2'
  }

  parameters {
    string(name: 'SECRET_NAME', defaultValue: '', description: 'Enter the secret name')
    string(name: 'SECRET_VALUE', defaultValue: '', description: 'Enter the secret value')
    string(name: 'PARAMETER_NAME', defaultValue: '', description: 'Enter the parameter store name')
    string(name: 'PARAMETER_VALUE', defaultValue: '', description: 'Enter the parameter store value')
  }

  stages {
    stage('Create Secrets and Parameters') {
      steps {
        script {
          def secretName = params.SECRET_NAME
          def secretValue = params.SECRET_VALUE
          def parameterName = params.PARAMETER_NAME
          def parameterValue = params.PARAMETER_VALUE

          withAWS(region: env.AWS_REGION, credentials: 'aws_creds') {
            mask("**${secretValue}**") {
              sh "aws secretsmanager create-secret --name ${secretName} --secret-string ${secretValue}"
            }
            mask("**${parameterValue}**") {
              sh "aws ssm put-parameter --name ${parameterName} --value ${parameterValue} --type String"
            }
          }
        }
      }
    }
  }
}
