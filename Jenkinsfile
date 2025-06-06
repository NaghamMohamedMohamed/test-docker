// pipeline {
//   agent {
//     kubernetes {
//       yamlFile 'kaniko-builder.yaml'  // path to your YAML file
//       defaultContainer 'kaniko'
//     }
//   }
//   stages {
//     stage('Build and Push Docker Image with Kaniko') {
//       steps {
//         container('kaniko') {
//           sh '''
//             /kaniko/executor \
//               --context `pwd` \
//               --dockerfile `pwd`/Dockerfile \
//               --destination=576607007321.dkr.ecr.us-east-1.amazonaws.com/gitops-gp-ecr/my-kaniko-image:latest \
//               --verbosity=info
//           '''
//         }
//       }
//     }
//   }
// }

podTemplate(yamlFile: 'kaniko-builder.yaml') {
  node(POD_LABEL) {
    container('kaniko') {
      echo "Kaniko build triggered"
    }
  }
}
