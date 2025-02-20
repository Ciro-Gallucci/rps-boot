### Forked to run a pipeline in Jenkins with PMD/FindSecBugs. The pipeline generates "PMD/FindSecBugs report" and a "PMD Warnings" section.

Pipeline configuration:
- type of job: pipeline
- definition: pipeline script from SCM
- SCM: git
- repository URL: `https://github.com/Ciro-Gallucci/rps-boot.git`
- branch: `*/master`
- script path: `Jenkinsfile`

Plugin needed:
- Warnings

Please note.
- FindSecBugs: just run the pipeline.
- PMD: copy `pom.xml` and `Jenkinsfile` to the main path (here currenlty the files for FindSecBugs).
