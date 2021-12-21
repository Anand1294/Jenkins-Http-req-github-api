import groovy.json.JsonSlurper

pipeline {
    agent any
    stages {
        stage ('HTTP Request to GitHub') {
            steps {
                script {
                    def response = httpRequest url: 'https://api.github.com/user',
                    authentication: 'anand-github-userid'//, consoleLogResponseBody: true
                    //def jsonResult = new JsonSlurper().parseText(response.content)
                    def jsonResult = readJSON text: response.content
                    def username = jsonResult.login
                    println("User ID : "+username)
                    //to search all repos using search api
                    def repoResponse = httpRequest authentication: 'anand-github-userid', url: "https://api.github.com/search/repositories?q=user:${username}"//, consoleLogResponseBody: true
                    //def repoResponse = httpRequest authentication: 'anand-github-userid', url: "https://api.github.com/users/${username}/repos?access_token=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", consoleLogResponseBody: true
                    def repoJson = readJSON text: repoResponse.content
                    //to parse json response using users api
                    /*repoJson.each {
                        println("Repo Name: ${it.full_name}")
                        //println("Is repo a Private Repo: ${it.private}")
                    }*/
                    //to parse json response using search api , it will display all the repo list
                    /*repoJson.items.each {
                        println("Full name: ${it.full_name}, Is repo a Private Repo: ${it.private}")
                    }*/
                    
                    //to parse json response using search api based on the match, it will display all the private repo list
                    repoJson.items.findAll {
                        it.private //!it.private : to fetch only public repos 
                    }.each {
                        println("Full name: ${it.full_name}, Is repo a Private Repo: ${it.private}")
                    }
                }
            }
        }
    }
}
