# Creating Chatbot with Bot Framework and Bot Service 

## Environment 
- Mac OS 

## Prerequisites 
- [Visual Studio 2017](https://visualstudio.microsoft.com/ko/downloads/)
  - Extensions : Bot Builder Template for Visual Studio 
  - 안타깝게도 Mac에서는 안되는 것 같다... 
    - 이러한 오픈 소스도 있음. [botframework-template-vs-for-mac](https://github.com/User1m/botframework-template-vs-for-mac)
- Azure Account
- [BotFramework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)  

## Language support
- C# and Node. 이 글에서는 C#으로. 

## Web App Bot
- Bot Framework + Azure Storage => Bot Service 를 통틀어.
- 다양한 매신저 Channel들 지원: Skype, Telegram, Slack, Facebook... 

## Steps to build the bot
### 1) ADD Resource Group
- Resource Group 이름은 내 계정 안에서만 Unique 
### 2) 생성한 Resource Group에서 Web App Bot 생성(create)
- Pricing tier
  - F0 : Free
  - S1 : Standard
### 3) Visual Studio 에서 Project 생성
1. extension 을 이용한 Bot builder template 이용 
2. Azure Portal > 봇 > Build > download bot source code 하여 폴더 내 `.sln` 파일 더블 클릭
![download_bot_source_code](./download_bot_source_code.png)
  - `MessageReceivedAsync` 메서드 : 실제 대화를 처리하는 부분
  - 상단 실행 버튼을 통해 실행 > `http://localhost:3984`
  
3. Azure CLI로 코드 다운로드 
  - 위 2 방법을 CLI로 
  ```
  $ az bot download --name "my-bot-name" --resource-group "my-resource-group"
  ```
### 3-1) Online Editor 이용
- Bot Web App > Build > 아래 'Open online code editor'를 통해 코드 수정 후 바로 git commit/push를 통해 수정 가능 
![online editor](./online_editor.png)
### 4) Emulator에서 테스트 
- New Bot Configuration 을 통해 설정 
![bot configuration](./new_bot_config_emulator.png) 
### 5) 게시(Publish)
- Window 환경에서는 Visual Studio에서 우클릭을 통해 바로 publish 가능한 것으로 보이나 Mac Community 버전에서는 보이지 않음. ([Professional로 하면 보이는 것 같음](https://stackoverflow.com/questions/44750522/publish-to-azure-in-visual-studio-community-for-mac))
  - 대신 [Azure CLI](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-tools-az-cli?view=azure-bot-service-3.0#7-publish-to-azure-from-the-cli)를 이용해 publish. (처음 azure cli 쓴다면 [이 노트](./azure_cli.md) 참고)
  ```
  $ az bot publish --name "my-bot-name" --resource-group "my-resource-group" // 처음 배포 경우 
  $ az bot update --name "my-bot-name" --resource-group "my-resource-group" // 이미 만들어진 앱인 경우 
  ```
### 6) Azure Portal 에서 확인
- Test in Web Chat 에서 로컬과 같이 동작하는지 확인 

## Deploy 
- Continuous Delivery using Github : https://docs.microsoft.com/en-us/azure/bot-service/bot-service-build-continuous-deployment?view=azure-bot-service-4.0 

## Troubleshooting 

## Sample Apps
- [mycheesebot](https://github.com/jiyeonseo/mycheesebot) - nodejs 

## More resources 
- Azure Dev Doc(SDK 4.x) : https://docs.microsoft.com/en-us/azure/bot-service/?view=azure-bot-service-4.0 
