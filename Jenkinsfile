#!/usr/bin/env groovy
// Define variables
List category_list = ["\"Select:selected\"","\"Vegetables\"","\"Fruits\""]
List fruits_list = ["\"Select:selected\"","\"apple\"","\"banana\"","\"mango\""]
List vegetables_list = ["\"Select:selected\"","\"potato\"","\"tomato\"","\"broccoli\""]
List default_item = ["\"Not Applicable\""]
String categories = buildScript(category_list)
String vegetables = buildScript(vegetables_list)
String fruits = buildScript(fruits_list)
String items = populateItems(default_item,vegetables_list,fruits_list)
// Methods to build groovy scripts to populate data
String buildScript(List values){
  return "return $values"
}
String populateItems(List default_item, List vegetablesList, List fruits
List){
return """if(Categories.equals('Vegetables')){
     return $vegetablesList
     }
     else if(Categories.equals('Fruits')){
     return $fruitsList
     }else{
     return $default_item
     }
     """
}
// Properties step to set the Active choice parameters via 
// Declarative Scripting
properties([
    parameters([
        [$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT',   name: 'Categories', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["ERROR"]'], script: [classpath: [], sandbox: false,
        script:  categories]]],
[$class: 'CascadeChoiceParameter', choiceType: 'PT_SINGLE_SELECT',name: 'Items', referencedParameters: 'Categories', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], sandbox: false, script: 'return ["error"]'], script: [classpath: [], sandbox: false, script: items]]]
    ])
])
pipeline {
    agent { 
           node {
               label 'docker'
            }
          } 

   parameters {
  string(name: 'package', defaultValue: '', description: 'Packages to be installed')
  extendedChoice bindings: '', description: '', groovyClasspath: '', groovyScript: '''def choices=[]
    textFile= new File("/tmp/sam1.txt")
    textFile.eachLine{
    choices.add(it)
}
return choices''', multiSelectDelimiter: ',', name: 'testing', quoteValue: false, saveJSONParameterToFile: false, type: 'PT_SINGLE_SELECT', visibleItemCount: 5
}

    
        
    stages {
        stage('workspace') {
            steps {
           
            sh 'ls -l && echo $testing'
            
            }
        }
        
        stage('Test') {
            steps {
        sh 'echo Running terraform'
        sh 'echo ${Categories}'

     
    }
        }
        stage('Triggering') {
          steps {
              ansibleTower extraVars: 'packs: "${package}"', jobTemplate: 'running-locals', jobType: 'run', throwExceptionWhenFail: false, towerCredentialsId: '8fb7ead4-0d33-4b1a-b6af-e605e915aa32', towerLogLevel: 'false', towerServer: 'localtower'
          }
        }
    }
        post {
            always {
                cleanWs()
            }
        }
    
    }