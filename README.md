# Basic Script
```
pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                echo "build..."
            }
        }
    }
}
```
# POST 
Execute logic after all stages executed 
```
post{
    always{
        // execuet even fail or success
    }
    success{
        // only buil success
    }
    failure{
        // if build fail
    }
}
```

# Conditions
```
pipeline {
    agent any 
    stages {
        stage('Build') { 
            when {
                expression {
                    BRANCH_NAME == 'dev' || BRANCH_NAME == 'master'
                }
            }
            // these steps only eecute if above is true
            steps {
                echo "build..."
            }
        }
    }
}
```

# Enviroment Variables

## Check all the by default available variable in Jenkins
`http://localhost:8080/env-vars.html/`

## Define Enviroment Variables
```
pipeline {
    agent any 
    enviroment{
        // these will available in every stages
        NEW_VERSION = '1.3.0'
    }
    stages {
        stage('Build') { 
            steps {
                echo "build..."
                echo "Building version ${NEW_VERSION}"
            }
        }
    }
}
```

# Using Credintials
First define credentials and then follow below
```
enviroment{
        CREDENTIALS = credentials('id')
    }
```

```
withCredentials([
    usernamePassword(credentials: 'id', usernameVariable: USER, passwordVariable: PWD)
]){
    echo "User ${USER}"
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
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy')
        choice(name: 'VERSION', choices: [ '1.0', '1.2'] , description: 'new one')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
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
                echo "${VERSION}"
                echo "build..."
            }
        }
    }
}
```

# Write Groovy Scripts

```
def gv

pipeline {
    agent any 
    stages {
        stage('init') { 
            steps {
                script {
                    // write groovy script
                    gv = load "script.groovy"
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






# Reference
https://www.youtube.com/watch?v=7KCS70sCoK0&t=481s&ab_channel=TechWorldwithNana
