node {
    
stage('Checkout') {
      checkout scm
    }
stage('terraform install') {
    install_terraform()
  }   
    
stage('terraform init') {
    terraform_init()
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
    sript = '''
        set +x -e
         ${WORKSPACE}/terraform init -backend-config vars/${env}/s3.tfbackend
        '''
    sh(script: script, returnStatus: true)
}

def terraform_plan() {
    sript = '''
        set +x -e
         ${WORKSPACE}/terraform plan -var-file vars/${env}/terraform.tfvars -out tfplan
        '''
    sh(script: script, returnStatus: true)
}

def terraform_apply() {
    sript = '''
        set +x -e
         ${WORKSPACE}/terraform apply -auto-approve -var-file vars/${env}t/terraform.tfvars
        '''
    sh(script: script, returnStatus: true)
}
