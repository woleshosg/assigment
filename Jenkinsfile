node {
    
stage('Checkout') {
      checkout scm
    }
stage('terraform configure') {
    install_terraform()
    ansiColor('xterm') {
        terraform_init()
    }
  }   
    
stage('terraform plan') {
     ansiColor('xterm') {
        terraform_plan()
     }
      ansiColor('xterm') {
        input(id: "Deploy", message: "Approve Changes?", ok: 'Deploy')
      }
  }  
    
stage('terraform apply') {
     ansiColor('xterm') {
        terraform_apply()
     }
  }  
    

}

def install_terraform() {
    script = """
        set -x -e
        echo ${BUILD_TAG} ${WORKSPACE}
        wget -q https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip -O ${WORKSPACE}/terraform_${TF_VERSION}_linux_amd64.zip
        unzip -fo ${WORKSPACE}/terraform_${TF_VERSION}_linux_amd64.zip
        chmod +x ${WORKSPACE}/terraform
        ${WORKSPACE}/terraform -version
        """
    sh(script: script, returnStatus: true)
}

def terraform_init() {
    script = '''
        set +x -e
         ${WORKSPACE}/terraform init -backend-config vars/${env}/s3.tfbackend
        '''
    sh(script: script, returnStatus: true)
}

def terraform_plan() {
    script = '''
        set +x -e
         ${WORKSPACE}/terraform plan -var-file vars/${env}/terraform.tfvars -out tfplan
        '''
    sh(script: script, returnStatus: true)
}

def terraform_apply() {
    script = '''
        set +x -e
         ${WORKSPACE}/terraform apply -auto-approve -var-file vars/${env}/terraform.tfvars
        '''
    sh(script: script, returnStatus: true)
}
