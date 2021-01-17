def TIMESTAMP = Calendar.getInstance().getTime().format('YYYYMMdd-hhmm')

pipeline {
  agent {
    label "master"
  }

  environment {
    AGENT_NAME  = "netlify"
    AGENT_URL   = "https://stupefied-lamarr-1f8830.netlify.app/"
  }

  stages {


    stage("Validation") {
      steps {
        timeout (time: 15, unit: 'MINUTES') {
          input message: 'Are you sure you want to deploy the agent? (Click "Proceed" to continue...)'
        }
      }
    }


    stage("Initialize") {
      steps {
        sh(
          label: "Run agent deploy",
          script: """
            cd /srv/deployment/remax/agent
            ansible-playbook agent.yml --extra-vars "agent_name=${AGENT_NAME} agent_url=${AGENT_URL}"
          """
        )
      }
    }
  }
}
