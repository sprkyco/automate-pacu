pipeline {
   agent {
       docker { image 'python:latest'}
   }

   stages {
      stage('pre-build') {
         steps {
            ASSUME_ROLE_ARN="arn:aws:iam::880018799862:role/automate-pacu-assumed"
            TEMP_ROLE=`aws sts assume-role --role-arn $ASSUME_ROLE_ARN --role-session-name pacu-bot --duration-seconds 900`
            export TEMP_ROLE
            yum install -y expect git jq curl
            export AWS_ACCESS_KEY_ID=$(echo "${TEMP_ROLE}" | jq -r '.Credentials.AccessKeyId')
            export AWS_SECRET_ACCESS_KEY=$(echo "${TEMP_ROLE}" | jq -r '.Credentials.SecretAccessKey')
            export AWS_SESSION_TOKEN=$(echo "${TEMP_ROLE}" | jq -r '.Credentials.SessionToken')
            git clone https://github.com/RhinoSecurityLabs/pacu.git
            git clone https://github.com/boto/boto3.git
            git clone https://github.com/boto/botocore.git
            rm -rf pacu/boto3/ && rm -rf pacu/botocore/
            mv boto3/boto3/ pacu/
            mv botocore/botocore/ pacu/
            pip install -r pacu/requirements.txt
            mv pacu-* pacu/
         }
      }
   }
}
