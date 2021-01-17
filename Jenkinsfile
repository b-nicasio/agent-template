def TIMESTAMP = Calendar.getInstance().getTime().format('YYYYMMdd-hhmm')

pipeline {
  agent {
    label "master"
  }

  environment {
    NETLIFY_AUTH_TOKEN  = credentials('netlify-token')
    NETLIFY_DEPLOY_ID   = f3939897-1e6e-4b74-bbdb-e7e9b903bec8
    AGENT_NAME          = netlify
  }

  stages {
    stage("Environment") {
      steps {
        sh(
          label: "Obtaining netlify URL",
          script: """
            ${env.NETLIFY_URL} = netlify api getSite --data '{ "site_id": "${NETLIFY_DEPLOY_ID}"}' | grep -Po '"url":.*?[^\\]",' | head -n 1 | awk 'NR==1{print $2,$3}' | tr -d '",'
          """
        )
      }
    }

    stage("Validation") {
      steps {
        timeout (time: 15, unit: 'MINUTES') {
          input message: "Are you sure you want to deploy ${AGENT_NAME} ? (Click "Proceed" to continue...)"
        }
      }
    }


    stage("Initialize") {
      steps {
        sh(
          label: "Run agent deploy",
          script: """
            cd /srv/deployment/remax/agent
            ansible-playbook agent.yml --extra-vars "agent_name=${AGENT_NAME} agent_url=${env.NETLIFY_URL}"
          """
        )
      }
    }
  }
}
