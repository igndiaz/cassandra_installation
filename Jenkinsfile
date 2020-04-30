pipeline {
    agent any 
    parameters {
        string(name: 'CLUSTER_NAME', defaultValue: 'Test Cluster', description: 'Nombre del Cluster Cassandra')
        string(name: 'NODOS', defaultValue: '1', description: 'Cantidad de Nodos Cluster')
        string(name: 'SNITCH', defaultValue: 'SimpleSnitch', description: 'Tipo de Snitch Cluster')
    }
    stages {
        stage('Instalacion Cassandra') {
            steps {
                sh 'echo "deb https://downloads.apache.org/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list'
                sh 'curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -'
                sh 'sudo apt-get update'
                sh 'sudo apt-get install cassandra'
            }  
        }
        stage('Modificaciones Nodo') {
            steps {
                sh 'cat /etc/cassandra/cassandra.yaml | sed -e "s/'Test Cluster'/${params.SNITCH}/g;"'
            }   
        }  
        stage('Inicio de Servicio & Validación') {
            steps {
                sh 'sudo service cassandra start'
                sh 'nodetool status'
                sh 'cqlsh -e 'describe keyspaces;''
            }  
        }    
    }
}
