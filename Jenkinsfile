pipeline {
  agent any
  options { timestamps() }

  stages {

    stage('Checkout') {
      steps {
        echo '=== Checkout : récupération du code depuis GitHub ==='
        checkout scm
        bat 'git status'
      }
    }

    stage('Build') {
      steps {
        echo '=== Build : préparation du projet ==='
        bat 'dir'
        bat 'if not exist index.html exit 1'
        bat 'if not exist style.css exit 1'
        echo 'Les fichiers index.html et style.css existent'
      }
    }

    stage('Test') {
      steps {
        echo '=== Test : vérification du contenu ==='
        bat 'findstr /C:"<h1>" index.html'
        bat 'findstr /C:"style.css" index.html'
        bat 'findstr /C:"font-family" style.css'
        echo 'Tous les tests sont OK'
      }
    }

    stage('Delivery') {
      steps {
        echo '=== Delivery : livraison de l application ==='
        bat 'if not exist delivery mkdir delivery'
        bat 'copy index.html delivery\\index.html'
        bat 'copy style.css delivery\\style.css'
        archiveArtifacts artifacts: 'delivery/**', fingerprint: true
        echo 'Application livrée dans le dossier delivery'
      }
    }
  }

  post {
    success { echo 'Build OK' }
    failure { echo 'Build KO' }
  }
}