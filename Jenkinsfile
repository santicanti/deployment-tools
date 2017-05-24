node {
   stage('Preparation') {
      echo 'Getting packer if it doesnt exist...'
      if (!fileExists('./packer')) {
        sh 'wget https://releases.hashicorp.com/packer/1.0.0/packer_1.0.0_linux_amd64.zip'
	      sh 'unzip packer_1.0.0_linux_amd64.zip -d .'
        sh 'rm packer_1.0.0_linux_amd64.zip'
      }
   }
   stage('Checkout') {
      echo 'Getting the deployment tools source code...'
      checkout scm
   }
   stage('Copy package') {
      echo 'Copying package to current dir...'
      sh "cp $PACKAGE_PATH ./package.deb"
   }
   stage('Call packer') {
       echo 'Getting file name...'
       String filename = PACKAGE_PATH.take(PACKAGE_PATH.lastIndexOf('.')) + currentBuild.number

       sh "./packer build -var 'aws_access_key='$AWS_ACCESS_KEY -var 'aws_secret_key='$AWS_SECRET_KEY -var 'ami_name'=$filename template.json"
   }
}
