pipeline {
  agent any
  environment {
      AWS_REGION = 'eu-west-2'
  }

  stages {
    stage('User Input') {
      steps {

        script {
          def secretNameInput = input message: 'Enter Secret Name', parameters: [
            string(name: 'SECRET_NAME', defaultValue: '', description: 'Enter the secret name')
          ]
          def secretValueInput = input message: 'Enter Secret Value', parameters: [
            string(name: 'SECRET_VALUE', defaultValue: '', description: 'Enter the secret value')
          ]
          def parameterNameInput = input message: 'Enter Parameter Name', parameters: [
            string(name: 'PARAMETER_NAME', defaultValue: '', description: 'Enter the parameter store name')
          ]
          def parameterValueInput = input message: 'Enter Parameter Value', parameters: [
            string(name: 'PARAMETER_VALUE', defaultValue: '', description: 'Enter the parameter store value')
          ]


          secretName = secretNameInput.SECRET_NAME
          secretValue = secretValueInput.SECRET_VALUE
          parameterName = parameterNameInput.PARAMETER_NAME
          parameterValue = parameterValueInput.PARAMETER_VALUE
        }
      }
    }

    stage('Create Secrets and Parameters') {
      steps {

        withAWS(region: AWS_REGION, credentials: 'aws_creds') {
          sh "aws secretsmanager create-secret --name ${secretName} --secret-string ${secretValue}"
        }


        withAWS(region: awsRegion, credentials: 'my-credentials') {
          sh "aws ssm put-parameter --name ${parameterName} --value ${parameterValue} --type String"
        }
      }
    }
  }
}
