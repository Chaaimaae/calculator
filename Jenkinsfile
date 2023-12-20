pipeline {
 agent any
 stages {

 	stage("Compilation") {
 		steps {
 			sh "./gradlew compileJava"

 		}

 	}
 	stage("test unitaire") {
		steps {
			sh "./gradlew test"
		}
	}
	stage("Couverture du code") {
 		steps {
 			sh "./gradlew jacocoTestReport"
 			publishHTML (target: [reportDir: 'build/reports/jacoco/test/html',reportFiles: 'index.html',
			reportName: "JaCoCo Report"])
			sh "./gradlew jacocoTestCoverageVerification"
 		}
 	}
 	stage("Analyse statistique du code") {
 		steps {
 			sh "./gradlew checkstyleMain"
 			publishHTML (target: [
 			reportDir: 'build/reports/checkstyle/',
 			reportFiles: 'main.html',
 			reportName: "Checkstyle Report"
			])
 		}
 	}
 	stage("Package") {
 		steps {
 			sh "./gradlew build"

 		}
	}
	stage("Docker build") {
 		steps {
 			sh "docker build -t calculator ."

 		}
	}
	stage("Docker push") {
		steps {
			sh "docker push localhost:5000/calculator"
		}
	}
	stage("Déploiement sur staging") {
 		steps {
 			sh "docker run -d --rm -p 8765:8080 --name calculator localhost:5000/calculator"
 		}
	}
 }
 
 post {
	always {
		mail to: 'majidlearning7@gmail.com',
		subject: "Cher lion Votre compilation est terminée: ${currentBuild.fullDisplayName}",
		body: " Votre build est accompli, Veuilez vérifier: ${env.BUILD_URL}"
		}	
}

}
