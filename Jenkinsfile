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
                sh "echo 'deb https://downloads.apache.org/cassandra/debian 311x main' | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list"
                sh "curl https://downloads.apache.org/cassandra/KEYS | sudo apt-key add -"
                sh "sudo apt-get -y install cassandra"
            }  
        }
        stage('Modificaciones Nodo') {
            steps {
                sh "sudo sed -i 's/Test Cluster/${params.CLUSTER_NAME}/gI' /etc/cassandra/cassandra.yaml"
            }   
        }  
        stage('Inicio de Servicio & Validación') {
            steps {
                sh "sudo service cassandra start"
                sh "nodetool status"
                sh "cqlsh -e 'describe keyspaces;'"
            }  
        }    
    }
}
