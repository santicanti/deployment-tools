node {
  try {
    stage('Preparation') {
      echo 'Getting packer if it doesnt exist...'
      try {
        if (!fileExists('./packer')) {
          sh 'wget https://releases.hashicorp.com/packer/1.0.0/packer_1.0.0_linux_amd64.zip'
          sh 'unzip packer_1.0.0_linux_amd64.zip -d .'
          sh 'rm packer_1.0.0_linux_amd64.zip'
        }
      } catch(err) {
        throw new Exception("There was an error getting packer.")
      }
    }
    stage('Checkout') {
      echo 'Getting the deployment tools source code...'
      try {
        checkout scm
      } catch (err) {
        throw new Exception('There was an error during checkout.')
      }
    }
    stage('Copy package') {
      echo 'Copying package to current dir...'
      try {
        sh "cp $PACKAGE_PATH ./package.deb"
      } catch (err) {
        throw new Exception('There was an error copying the package.')
      }
    }
    stage('Call packer') {
      echo 'Creating ami name...'
      try {
        String fileName = PACKAGE_PATH.substring(PACKAGE_PATH.lastIndexOf('/') + 1)
        String amiName = fileName.take(fileName.lastIndexOf('.')) + currentBuild.number
      } catch (err) {
        throw new Exception('There was an error creating the ami name.')
      }

      echo 'Calling packer...'
      try {
        sh "./packer build -var 'aws_access_key='$AWS_ACCESS_KEY -var 'aws_secret_key='$AWS_SECRET_KEY -var 'ami_name'=$amiName template.json"
      } catch (err) {
        throw new Exception('There was an error calling packer.')
      }
    }
  } catch (err) {
    echo err.getMessage()
  } finally {
     deleteDir()
  }
}
