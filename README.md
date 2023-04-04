# Guidelines - Jenkins
## Basic Script
```
pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                echo "build stage..."
            }
        }
    }
}
```
## POST
This section can be used to execute logic after all stages have been executed 
```
post{
    always{
        // get executed even fail or success
    }
    success{
        // only get executed if the build was succesful
    }
    failure{
        // only get executed if the build was a failure
    }
}
```
## Enviroment Variables

### Check all the default enviroment variables
`http://localhost:8080/env-vars.html/`

### Define Enviroment Variables
```
pipeline {
    agent any 
    enviroment{
        VERSION = '1.3.0'
    }
    stages {
        stage('Build') { 
            steps {
                echo "Version: ${VERSION}"
            }
        }
    }
}
```
## Conditions
```
pipeline {
    agent any 
    stages {
        stage('Build') { 
            when {
                expression {
                    env.VERSION == '1.0' || BRANCH_NAME == 'master'
                }
            }
            // these steps only execute if above is true
            steps {
                echo "build..."
            }
        }
    }
}
```
# Using Credintials
Before trying this out we have to define credentials.
```
enviroment{
        CREDENTIALS = credentials('id')
    }
```
```
withCredentials([
    usernamePassword(credentialsId: 'nginx-credentials', usernameVariable: "USER", passwordVariable: "PWD")
]) {
    sh "echo 'Nginx Credentials: ${USER} | ${PWD}' >> index.html" 
} 
```

# Tools
```
pipeline {
    agent any 
    tools{
        maven 'Maven'
    }
}
```
# Parameters
```
pipeline {
    agent any 
    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'Description here')
        choice(name: 'VERSION', choices: [ '1.0', '1.2'] , description: 'Description here')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'Description here')
    }
    stages {
        stage('Build') { 
            when{
                expression{
                    params.executeTests
                }
            }
            // eexute only if executeTests to true
            steps {
                echo "${params.VERSION}"
                echo "build..."
            }
        }
    }
}
```
# Write Groovy Scripts
```
def groovyScript

pipeline {
    agent any 
    stages {
        stage('init') { 
            steps {
                script {
                    // write groovy script
                    groovyScript = load "script.groovy"
                }
            }
        }
        stage('Build') { 
            steps {
                script {
                    gv.buildApp()
                }
            }
        }
    }
}
```
## Triggers
We can use 'https://ngrok.com/download' to expose local Jenkins to the public

# Reference
https://www.youtube.com/watch?v=7KCS70sCoK0&t=481s&ab_channel=TechWorldwithNana
