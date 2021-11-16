def swarmManager = 'ec2-3-71-6-151.eu-central-1.compute.amazonaws.com'
def region = 'eu-central-1'
def swarmSshagentCredentials = 'swarm-sandbox'

node('master'){
    stage('Checkout'){
        checkout scm
    }

     sshagent (credentials: ["${swarmSshagentCredentials}"]){
         stage('Copy'){
            sh "scp -o StrictHostKeyChecking=no docker-compose.yml ec2-user@${swarmManager}:/home/ec2-user"
        }

        stage('Deploy stack'){
            sh "ssh -oStrictHostKeyChecking=no ec2-user@${swarmManager} '\$(\$(aws ecr get-login --no-include-email --region ${region}))' || true"
            sh "ssh -oStrictHostKeyChecking=no ec2-user@${swarmManager} docker stack deploy --compose-file docker-compose.yml --with-registry-auth watchlist"
        }
     }
}

