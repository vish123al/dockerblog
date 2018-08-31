def gitCommit() {
    sh "git rev-parse HEAD > GIT_COMMIT"
    def gitCommit = readFile('GIT_COMMIT').trim()
    sh "rm -f GIT_COMMIT"
    return gitCommit
}

node {

    // Checkout source code from Git
    stage 'Checking out scm for repository'
    checkout scm
    stage '(TEST) unit/integration testing'
    //sh 'make test'
//checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/checkstyle-result.xml', unHealthy: ''
//dry canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/cpd.xml', unHealthy: ''
//findbugs canComputeNew: false, defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', pattern: '**/findbugs.xml', unHealthy: ''
//pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/pmd.xml', unHealthy: ''
//warnings canComputeNew: false, canResolveRelativePaths: false, categoriesPattern: '', consoleParsers: [[parserName: 'PHP Runtime']], defaultEncoding: '', excludePattern: '', healthy: '', includePattern: '', messagesPattern: '', parserConfigurations: [[parserName: 'PHP Runtime', pattern: '.php']], unHealthy: '' 
   
    stage '(BUILD) building image'
    sh "docker build -t vishaldenge/dockerblog:${gitCommit()} ."
    sh "docker login -u vishaldenge -p 'v!sh@l123' "
    stage '(PUBLISH) Pushing the image '
    sh "docker push vishaldenge/dockerblog:${gitCommit()}"
     stage '(DEPLOY) Deploying the container'
    marathon(
        url: 'http://172.29.133.15:8080',
        forceUpdate: true,
        filename: 'marathon.json',
        appId: 'blog',
        docker: "vishaldenge/dockerblog:${gitCommit()}".toString()
    )
   
        //stage 'Collect test reports'
        
         sh 'touch reports/*.xml'
         junit '**/reports/*.xml'
       // step([$class: 'JUnitResultArchiver', testResults: '**/reports/*.xml'])
        stage 'Clean up'
       
                         
   

}
