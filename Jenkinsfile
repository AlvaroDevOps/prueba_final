pipeline {
  agent any
  environment {
      POSTGRES_PASSWOR_SECRET = credentials('POSTGRES_PASSWOR_ID')
  }
  stages {
    
    stage('verificando_rama') {
      agent {
            node {
                  label 'aws_node'
            }
      }
      steps {
        echo 'Pulling...  {env.BRANCH_NAME}'
      }
    }
    
    stage("borra_todo_antes_de_empezar") {
      agent {
            node {
                  label 'aws_node'
            }
      }
      steps {
        sh 'docker rm -f postgres'
        sh 'docker rm -f phppgadmin'
        sh 'docker network rm alvaro-network'
      }
    }
    
    stage("crear_red") {
      agent {
            node {
                  label 'aws_node'
            }
      }
      steps {
        sh 'docker network create -d bridge alvaro-network'
      }
    }
    
    stage("crear_postgres") {
      agent {
            node {
                  label 'aws_node'
            }
      }
      steps {
        sh 'docker run --name postgres -e POSTGRES_PASSWORD=${POSTGRES_PASSWOR_SECRET} --network alvaro-network --network-alias host-postgres -d postgres:11'
      }
    }
    
    stage("crear_phppgadmin") {
      agent {
            node {
                  label 'aws_node'
            }
      }
      steps {
        sh 'docker run --name phppgadmin -p 80:8080 -p 443:8443 --env DATABASE_HOST=host-postgres --network alvaro-network --network-alias host-phppgadmin -d bitnami/phppgadmin:7.13.0'
      }
    }
    
    stage("crear_bd_dvdrental") {
      agent {
            node {
                  label 'aws_node'
            }
      }
      steps {
        sh 'docker exec postgres psql -U postgres -c "CREATE DATABASE dvdrental"'
      }
    }
    
    stage("descarga_y_extrae_db") {
      agent {
            node {
                  label 'aws_node'
            }
      }
      steps {
        sh 'curl -O https://www.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip'
        sh 'unzip -o dvdrental.zip'
      }
    }
    
    stage("copia_e_importa_db") {
      agent {
            node {
                  label 'aws_node'
            }
      }
      steps {
        sh 'docker cp dvdrental.tar postgres:/var/lib/postgresql/data'
        sh 'docker exec postgres pg_restore -U postgres -d dvdrental /var/lib/postgresql/data/dvdrental.tar'
      }
    }
// Esto lo hace en local
    
    stage('verificando_rama') {
      steps {
        echo 'Pulling...  {env.BRANCH_NAME}'
      }
    }
    stage("borra_todo_antes_de_empezar") {
      steps {
        sh 'docker rm -f postgres'
        sh 'docker rm -f phppgadmin'
        sh 'docker network rm alvaro-network'
      }
    }
    stage("crear_red") {
      steps {
        sh 'docker network create -d bridge alvaro-network'
      }
    }
    stage("crear_postgres") {
      steps {
        sh 'docker run --name postgres -e POSTGRES_PASSWORD=${POSTGRES_PASSWOR_SECRET} --network alvaro-network --network-alias host-postgres -d postgres:11'
      }
    }
    stage("crear_phppgadmin") {
      steps {
        sh 'docker run --name phppgadmin -p 80:8080 -p 443:8443 --env DATABASE_HOST=host-postgres --network alvaro-network --network-alias host-phppgadmin -d bitnami/phppgadmin:7.13.0'
      }
    }
    stage("crear_bd_dvdrental") {
      steps {
        sh 'docker exec postgres psql -U postgres -c "CREATE DATABASE dvdrental"'
      }
    }
    stage("descarga_y_extrae_db") {
      steps {
        sh 'curl -O https://www.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip'
        sh 'unzip -o dvdrental.zip'
      }
    }
    stage("copia_e_importa_db") {
      steps {
        sh 'docker cp dvdrental.tar postgres:/var/lib/postgresql/data'
        sh 'docker exec postgres pg_restore -U postgres -d dvdrental /var/lib/postgresql/data/dvdrental.tar'
      }
    }


  }
}