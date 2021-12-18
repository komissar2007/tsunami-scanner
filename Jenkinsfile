import groovy.transform.Field
properties([ parameters([string(defaultValue: '127.0.0.1', description: 'list of IPs seperated by ","', name: 'list of IPs to scan')])])
@Field int failCount = 0

node('base-linux')
{
    try{
        def ipList = params['list of IPs to scan'].split(',')
        for (String ip: ipList) {
            runTsunamiScan(ip)
            checkScanStatus(ip)
        }
        dir('/home/jenkins/volume/tsunami-logs'){
            archiveArtifacts artifacts: '*.json'    
        }
        if (failCount>0){
            print("${failCount} / ${ipList.size()} got failed in scan, please check scan report ")
            currentBuild.result = 'FAILURE'
        }
        }
    catch(error){
        print("error during scanning, please check logs")
       currentBuild.result = 'FAILURE' 
    }    
   
    
}

def runTsunamiScan(ipAddress){
    sh "docker run --rm --network='host' --name tsunami-scanner-${ipAddress} -v /home/jenkins/volume/tsunami-logs:/usr/tsunami/logs slavarub/tsunami --ip-v4-target=${ipAddress} --scan-results-local-output-format=JSON --scan-results-local-output-filename=/usr/tsunami/logs/tsunami-${ipAddress}-output.json"
} 

def checkScanStatus(ipAddress){
    dir('/home/jenkins/volume/tsunami-logs'){
        reportFile = "tsunami-${ipAddress}-output.json"
        report = readJSON file: reportFile
        if (!report.scanStatus.equals('SUCCEEDED')){
            failCount++
        }
        print('report is: ' + report.scanStatus)
    }
    
} 