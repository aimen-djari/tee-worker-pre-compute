// Readme @ http://gitlab.iex.ec:30000/iexec/jenkins-library
@Library('global-jenkins-library@1.6.2') _

def nativeImage = buildSimpleDocker_v2(dockerfileDir: './docker', buildContext: '.',
        dockerImageRepositoryName: 'tee-worker-pre-compute', imageprivacy: 'public')
sconeBuildAllTee(nativeImage: nativeImage, targetImageRepositoryName: 'tee-worker-pre-compute',
        sconifyArgsPath: './docker/sconify.args')
