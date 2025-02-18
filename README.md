### Forked to run a pipeline in Jenkins with PMD. The pipeline generates "PMD report" and a "PMD Warnings" section.

Pipeline configuration:
- type of job: pipeline
- definition: pipeline script from SCM
- SCM: git
- repository URL: https://github.com/Ciro-Gallucci/rps-boot.git
- branch: */master
- script path: Jenkinsfile
