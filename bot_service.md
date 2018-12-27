# Creating Chatbot with Bot Framework and Bot Service 

## Prerequisites 
- IDE : [Visual Studio 2017](https://visualstudio.microsoft.com/ko/downloads/)
  - Extensions : Bot Builder Template for Visual Studio 
  - 안타깝게도 Mac에서는 안되는 것 같다... 
    - 이러한 오픈 소스도 있음. https://github.com/User1m/botframework-template-vs-for-mac 
- Azure Account
- [BotFramework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)  

## Language support
- C# and Nodejs
- 이 글에서는 C#으로. 

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
![download_bot_source_code](https://github.com/jiyeonseo-study/Azure-study/blob/master/download_bot_source_code.png)
  - `MessageReceivedAsync` 메서드 : 실제 대화를 처리하는 부분
  - 상단 실행 버튼을 통해 실행 
### 4) Emulator에서 테스트 
- New Bot Configuration 을 통해 설정 
![bot configuration]

## Deploy 
- Continuous Delivery using Github : https://docs.microsoft.com/en-us/azure/bot-service/bot-service-build-continuous-deployment?view=azure-bot-service-4.0 

## Sample Apps
- [mycheesebot](https://github.com/jiyeonseo/mycheesebot) - nodejs 

## More resources 
- Azure Dev Doc(SDK 4.x) : https://docs.microsoft.com/en-us/azure/bot-service/?view=azure-bot-service-4.0 
