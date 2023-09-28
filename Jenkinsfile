@Library('global-jenkins-library@2.7.3') _

String repositoryName = 'tee-worker-pre-compute'

buildInfo = getBuildInfo()

// add parameters for non-PR builds when branch is not develop or production branch
boolean addParameters = !buildInfo.isPullRequestBuild && !buildInfo.isDevelopBranch && !buildInfo.isProductionBranch

// Override properties defined in getBuildInfo and add parameters
if (addParameters) {
  properties([
    buildDiscarder(logRotator(numToKeepStr: '10')),
    parameters([booleanParam(description: 'Build TEE images', name: 'BUILD_TEE')])
  ])
}

buildJavaProject(
        buildInfo: buildInfo,
        integrationTestsEnvVars: [],
        shouldPublishJars: false,
        shouldPublishDockerImages: true,
        dockerfileDir: '.',
        buildContext: '.',
        dockerImageRepositoryName: repositoryName)

// BUILD_TEE parameter only exists if addParameters is true
// If BUILD_TEE is false, TEE builds won't be executed and we return here
if (addParameters && !params.BUILD_TEE) {
    return
}

sconeBuildUnlocked(
        nativeImage:     "docker-regis.iex.ec/$repositoryName:$buildInfo.imageTag",
        imageName:       repositoryName,
        imageTag:        buildInfo.imageTag,
        sconifyArgsPath: './docker/sconify.args',
        sconifyVersion:  '5.7.1'
)
