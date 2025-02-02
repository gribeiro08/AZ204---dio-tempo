# AZ204---dio-tempo

Estava realizando os exemplos com a conta corporativa de estudante, acontece que com essa conta eu nãwo posso utilizar o azure devops, pois a organização bloqueia.
Consegui acompanhar até o momento de gerar a imagem, após isso não consegui conectar o Docker com o Azure Devops para gerar a imagem.
Segue o codigo Yaml
# Starter pipeline
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml

trigger:
- main

pool:
  vmImage: ubuntu-latest

variables:
  solution: './APItempoDIO/*.sln'
  buildPlataform: 'Any CPU'
  buildConfiguration: 'Release'

steps:

- task: UseDotNet@2
  displayName: 'Instal .Net SDK'
  inputs:
    packageType: 'sdk'
    version: '8.x'

- script: dotnet restore $(solution)
  displayName: 'Restore Solution'

- script: dotnet build $(solution) --configuration $(buildConfiguration)
  displayName: 'Build Solution'

- script: dotnet test $(solution) --configuration $(buildConfiguration) --no-build --collect:'XPlat Code Coverage'
  displayName: 'Build Solution'
 
- task: Docker@2
  inputs:
    containerRegistry: 'acrapidemocursoaz204'
    repository: 'api dio test'
    command: 'buildAndPush'
    Dockerfile: '.APItempoDIO/APItempoDIO/Dockerfile'
